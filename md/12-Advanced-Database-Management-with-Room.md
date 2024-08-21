# Lesson 12: Advanced Database Management with Room

In this lesson, we will delve deeper into advanced database management using Room in Android. We will cover defining relationships between entities, using Data Access Objects (DAOs) for data access, implementing migrations, testing database interactions, and handling complex queries.

## 1. Defining Relationships Between Entities

Room allows you to define relationships between entities using annotations. The most common relationships are one-to-one, one-to-many, and many-to-many.

### One-to-Many Relationship

In a one-to-many relationship, one entity can be associated with multiple instances of another entity. For example, a `User` can have multiple `Posts`.

#### Defining Entities

```kotlin
@Entity(tableName = "users")
data class User(
    @PrimaryKey(autoGenerate = true) val userId: Long = 0,
    val username: String
)

@Entity(tableName = "posts",
        foreignKeys = [ForeignKey(entity = User::class,
                                  parentColumns = ["userId"],
                                  childColumns = ["userId"],
                                  onDelete = ForeignKey.CASCADE)])
data class Post(
    @PrimaryKey(autoGenerate = true) val postId: Long = 0,
    val userId: Long,
    val content: String
)
```

### One-to-One Relationship

In a one-to-one relationship, one entity is associated with exactly one instance of another entity. For example, a `User` can have one `Profile`.

#### Defining Entities

```kotlin
@Entity(tableName = "profiles")
data class Profile(
    @PrimaryKey val userId: Long,
    val bio: String,
    val avatarUrl: String
)
```

### Many-to-Many Relationship

In a many-to-many relationship, multiple instances of one entity can be associated with multiple instances of another entity. This is typically implemented using a junction table.

#### Defining Entities and Junction Table

```kotlin
@Entity(tableName = "tags")
data class Tag(
    @PrimaryKey(autoGenerate = true) val tagId: Long = 0,
    val name: String
)

@Entity(primaryKeys = ["postId", "tagId"])
data class PostTagCrossRef(
    val postId: Long,
    val tagId: Long
)
```

## 2. Using DAO for Data Access

Data Access Objects (DAOs) are interfaces that define methods for accessing the database. Room generates the implementation of these methods at compile time.

### Creating the DAO

```kotlin
@Dao
interface UserDao {
    @Insert
    suspend fun insertUser(user: User)

    @Insert
    suspend fun insertPost(post: Post)

    @Transaction
    @Query("SELECT * FROM users WHERE userId = :userId")
    suspend fun getUserWithPosts(userId: Long): UserWithPosts
}
```

### Using LiveData and Flow

Room supports LiveData and Kotlin Flow for observing data changes. This allows your UI to react to database changes automatically.

#### Using LiveData

```kotlin
@Query("SELECT * FROM users")
fun getAllUsers(): LiveData<List<User>>
```

#### Using Flow

```kotlin
@Query("SELECT * FROM posts")
fun getAllPosts(): Flow<List<Post>>
```

## 3. Implementing Migrations

When you change the database schema (e.g., adding a new column), you need to handle migrations to ensure existing users can still access their data.

### Defining a Migration

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(database: SupportSQLiteDatabase) {
        // Add a new column to the users table
        database.execSQL("ALTER TABLE users ADD COLUMN email TEXT")
    }
}
```

### Adding Migration to the Database Builder

When building the Room database, you can add the migration:

```kotlin
val db = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "app_database"
).addMigrations(MIGRATION_1_2)
 .build()
```

## 4. Testing Database Interactions

Testing your database interactions is essential to ensure that your data access methods work correctly. You can use Room's in-memory database for testing.

### Setting Up In-Memory Database

```kotlin
val db = Room.inMemoryDatabaseBuilder(context, AppDatabase::class.java).build()
```

### Writing Tests

You can write tests using JUnit to verify your DAO methods:

```kotlin
@Test
fun insertAndGetUser() = runBlocking {
    val user = User(username = "testUser")
    userDao.insertUser(user)

    val retrievedUser = userDao.getUserWithPosts(user.userId)
    assertEquals(user.username, retrievedUser.username)
}
```

## 5. Handling Complex Queries

Room allows you to perform complex queries using SQL. You can use various SQL clauses to filter, sort, and aggregate data.

### Example of a Complex Query

```kotlin
@Query("SELECT * FROM posts WHERE userId = :userId ORDER BY postId DESC LIMIT :limit OFFSET :offset")
suspend fun getPostsByUser(userId: Long, limit: Int, offset: Int): List<Post>
```

### Using Joins

You can also use joins to retrieve related data from multiple tables:

```kotlin
@Transaction
@Query("SELECT * FROM users")
suspend fun getUsersWithPosts(): List<UserWithPosts>
```

### Data Class for Joined Results

Define a data class to hold the results of a join:

```kotlin
data class UserWithPosts(
    @Embedded val user: User,
    @Relation(
        parentColumn = "userId",
        entityColumn = "userId"
    )
    val posts: List<Post>
)
```

## Conclusion

In this lesson, we explored advanced database management with Room in Android. We learned how to define relationships between entities, use DAOs for data access, implement migrations, test database interactions, and handle complex queries. Mastering these concepts will enable you to create robust and efficient data management solutions in your Android applications. In the next lesson, we will delve into testing and debugging best practices to ensure the quality of your applications.