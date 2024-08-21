# Lesson 8: Data Persistence: Shared Preferences, DataStore, and Room

In this lesson, we will explore data persistence in Android applications. We will cover three primary methods for storing data: Shared Preferences, DataStore, and Room. Each method serves different use cases, and understanding when to use each will help you manage data effectively in your apps.

## 1. Storing Key-Value Pairs

### Shared Preferences
Shared Preferences is a simple way to store small amounts of primitive data as key-value pairs. It is typically used for storing user settings or preferences.

#### Implementing Shared Preferences

1. **Saving Data**
   To save data using Shared Preferences, follow these steps:

   ```kotlin
   // Get the SharedPreferences instance
   val sharedPreferences = getSharedPreferences("MyPrefs", MODE_PRIVATE)

   // Create an editor to write data
   val editor = sharedPreferences.edit()
   editor.putString("username", "JohnDoe")
   editor.putInt("userAge", 30)
   editor.apply() // or editor.commit() for synchronous saving
   ```

2. **Retrieving Data**
   You can retrieve stored data as follows:

   ```kotlin
   val username = sharedPreferences.getString("username", "defaultUsername")
   val userAge = sharedPreferences.getInt("userAge", 0) // Default value is 0
   ```

3. **Removing Data**
   To remove a specific key-value pair:

   ```kotlin
   editor.remove("username")
   editor.apply()
   ```

4. **Clearing All Data**
   To clear all data in Shared Preferences:

   ```kotlin
   editor.clear()
   editor.apply()
   ```

### DataStore
DataStore is a modern data storage solution that provides a more robust and type-safe way to store data compared to Shared Preferences. It uses Kotlin coroutines and Flow for asynchronous operations.

#### Implementing DataStore

1. **Adding Dependencies**
   First, add the DataStore dependency in your `build.gradle` file:

   ```groovy
   dependencies {
       implementation "androidx.datastore:datastore-preferences:1.0.0"
   }
   ```

2. **Creating DataStore**
   Create a singleton instance of DataStore:

   ```kotlin
   import androidx.datastore.preferences.core.Preferences
   import androidx.datastore.preferences.preferencesDataStore
   import kotlinx.coroutines.flow.Flow

   val Context.dataStore by preferencesDataStore(name = "settings")
   ```

3. **Saving Data**
   Use the following code to save data:

   ```kotlin
   import androidx.datastore.preferences.core.edit
   import androidx.datastore.preferences.core.stringPreferencesKey
   import kotlinx.coroutines.runBlocking

   val USERNAME_KEY = stringPreferencesKey("username")

   fun saveUsername(username: String) {
       runBlocking {
           context.dataStore.edit { preferences ->
               preferences[USERNAME_KEY] = username
           }
       }
   }
   ```

4. **Retrieving Data**
   To retrieve data, use a Flow:

   ```kotlin
   fun getUsername(): Flow<String> {
       return context.dataStore.data
           .map { preferences ->
               preferences[USERNAME_KEY] ?: "defaultUsername"
           }
   }
   ```

## 2. Implementing Room for Local Database Management

Room is an abstraction layer over SQLite that provides a more robust and easier way to manage local databases in Android applications. It allows you to define your database schema using annotations and provides compile-time checks for SQL queries.

### Setting Up Room

1. **Adding Dependencies**
   Add the Room dependencies in your `build.gradle` file:

   ```groovy
   dependencies {
       implementation "androidx.room:room-runtime:2.5.0"
       kapt "androidx.room:room-compiler:2.5.0" // For Kotlin
   }
   ```

2. **Creating Entity Classes**
   Define your data model as an entity. Each entity represents a table in the database.

   ```kotlin
   import androidx.room.Entity
   import androidx.room.PrimaryKey

   @Entity(tableName = "users")
   data class User(
       @PrimaryKey(autoGenerate = true) val id: Long = 0,
       val username: String,
       val age: Int
   )
   ```

3. **Creating Data Access Object (DAO)**
   Define an interface for data access methods.

   ```kotlin
   import androidx.room.Dao
   import androidx.room.Insert
   import androidx.room.Query

   @Dao
   interface UserDao {
       @Insert
       suspend fun insert(user: User)

       @Query("SELECT * FROM users")
       suspend fun getAllUsers(): List<User>

       @Query("DELETE FROM users WHERE id = :userId")
       suspend fun deleteById(userId: Long)
   }
   ```

4. **Creating the Database**
   Create the Room database class that extends `RoomDatabase`.

   ```kotlin
   import androidx.room.Database
   import androidx.room.Room
   import androidx.room.RoomDatabase
   import android.content.Context

   @Database(entities = [User::class], version = 1)
   abstract class AppDatabase : RoomDatabase() {
       abstract fun userDao(): UserDao

       companion object {
           @Volatile
           private var INSTANCE: AppDatabase? = null

           fun getDatabase(context: Context): AppDatabase {
               return INSTANCE ?: synchronized(this) {
                   val instance = Room.databaseBuilder(
                       context.applicationContext,
                       AppDatabase::class.java,
                       "app_database"
                   ).build()
                   INSTANCE = instance
                   instance
               }
           }
       }
   }
   ```

### Performing CRUD Operations

1. **Create (Insert)**
   To insert a new user:

   ```kotlin
   val user = User(username = "JohnDoe", age = 30)
   val userDao = AppDatabase.getDatabase(context).userDao()
   userDao.insert(user)
   ```

2. **Read (Query)**
   To retrieve all users:

   ```kotlin
   val users = userDao.getAllUsers()
   ```

3. **Update**
   To update a user, you can add an update method in the DAO:

   ```kotlin
   @Update
   suspend fun update(user: User)
   ```

4. **Delete**
   To delete a user by ID:

   ```kotlin
   userDao.deleteById(userId)
   ```

### Handling Migrations
When you change the database schema (e.g., adding a new column), you need to handle migrations to ensure existing users can still access their data.

1. **Define Migration**
   Create a migration object:

   ```kotlin
   val MIGRATION_1_2 = object : Migration(1, 2) {
       override fun migrate(database: SupportSQLiteDatabase) {
           // Perform migration steps
           database.execSQL("ALTER TABLE users ADD COLUMN email TEXT")
       }
   }
   ```

2. **Add Migration to Database Builder**
   Add the migration when building the database:

   ```kotlin
   Room.databaseBuilder(
       context.applicationContext,
       AppDatabase::class.java,
       "app_database"
   ).addMigrations(MIGRATION_1_2)
   ```

## Conclusion

In this lesson, we explored data persistence techniques in Android applications using Shared Preferences, DataStore, and Room. We learned how to store key-value pairs, manage local databases, perform CRUD operations, and handle migrations. Understanding these data persistence methods will help you effectively manage user data in your Android applications. In the next lesson, we will dive into networking fundamentals and how to make API calls in your app.