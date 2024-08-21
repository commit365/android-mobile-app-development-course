# Lesson 11: Working with APIs and Authentication

In this lesson, we will explore how to consume RESTful APIs in Android applications, handle JSON data through serialization and deserialization, and implement user authentication using OAuth and JWT. We will also discuss secure storage of credentials using EncryptedSharedPreferences.

## 1. Consuming RESTful APIs

### Understanding RESTful APIs

RESTful APIs allow you to interact with web services using standard HTTP methods (GET, POST, PUT, DELETE). They typically return data in JSON format, which can be easily consumed by Android applications.

### Making API Calls

To consume a RESTful API, you can use Retrofit, as discussed in the previous lesson. Here’s a quick recap of how to set up Retrofit for API calls:

1. **Define the API Interface**

```kotlin
interface ApiService {
    @GET("users")
    fun getUsers(): Call<List<User>>

    @POST("login")
    fun login(@Body loginRequest: LoginRequest): Call<LoginResponse>
}
```

2. **Create the Retrofit Instance**

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build()

val apiService = retrofit.create(ApiService::class.java)
```

3. **Making API Calls**

To make an API call, use the defined methods in your `ApiService`:

```kotlin
apiService.getUsers().enqueue(object : Callback<List<User>> {
    override fun onResponse(call: Call<List<User>>, response: Response<List<User>>) {
        if (response.isSuccessful) {
            val users = response.body()
            // Handle the list of users
        } else {
            // Handle error response
        }
    }

    override fun onFailure(call: Call<List<User>>, t: Throwable) {
        // Handle failure
    }
})
```

### Handling JSON Data

When you receive a JSON response, you can use Gson (which is integrated with Retrofit) to automatically convert the JSON into Kotlin objects.

#### Example Data Classes

Define your data classes to match the structure of the JSON response:

```kotlin
data class User(
    val id: Int,
    val name: String,
    val email: String
)

data class LoginRequest(
    val username: String,
    val password: String
)

data class LoginResponse(
    val token: String,
    val user: User
)
```

### Serialization and Deserialization

Retrofit automatically handles serialization and deserialization of JSON data using Gson. When you make a request, Retrofit converts the JSON response into the specified Kotlin data class.

## 2. Implementing User Authentication

### Understanding Authentication

Authentication is the process of verifying the identity of a user. Common methods for authentication include OAuth and JWT (JSON Web Tokens).

### OAuth Authentication

OAuth is a widely used authorization framework that allows users to grant third-party applications access to their resources without sharing their credentials.

1. **Setting Up OAuth**

To implement OAuth, you typically need to register your application with the OAuth provider (e.g., Google, Facebook) to obtain client credentials (client ID and client secret).

2. **Making OAuth Requests**

You would make a request to the OAuth provider to obtain an access token. Here’s an example of how to initiate an OAuth login flow:

```kotlin
val loginRequest = LoginRequest(username, password)
apiService.login(loginRequest).enqueue(object : Callback<LoginResponse> {
    override fun onResponse(call: Call<LoginResponse>, response: Response<LoginResponse>) {
        if (response.isSuccessful) {
            val token = response.body()?.token
            // Store the token securely
        }
    }

    override fun onFailure(call: Call<LoginResponse>, t: Throwable) {
        // Handle failure
    }
})
```

### JWT Authentication

JWT is a compact, URL-safe means of representing claims to be transferred between two parties. It is often used for authentication in web applications.

1. **Receiving the JWT**

When a user logs in, the server responds with a JWT that contains user information and claims. Store this token securely for future requests.

2. **Using the JWT for Subsequent Requests**

Include the JWT in the Authorization header for subsequent API calls:

```kotlin
val token = "Bearer $jwtToken"

val request = Request.Builder()
    .url("https://api.example.com/protected/resource")
    .addHeader("Authorization", token)
    .build()

val response = client.newCall(request).execute()
```

## 3. Secure Storage of Credentials

### Using EncryptedSharedPreferences

To securely store sensitive information such as tokens or user credentials, use `EncryptedSharedPreferences`. This ensures that data is encrypted and stored securely.

1. **Add Dependencies**

Add the following dependencies to your `build.gradle` file:

```groovy
dependencies {
    implementation "androidx.security:security-crypto:1.1.0-alpha03"
}
```

2. **Setting Up EncryptedSharedPreferences**

Create an instance of `EncryptedSharedPreferences`:

```kotlin
val sharedPreferences = EncryptedSharedPreferences.create(
    "secure_prefs",
    MasterKey.DEFAULT_MASTER_KEY_ALIAS,
    context,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)
```

3. **Storing Credentials Securely**

Store the JWT token securely:

```kotlin
val editor = sharedPreferences.edit()
editor.putString("jwt_token", token)
editor.apply()
```

4. **Retrieving Credentials**

Retrieve the stored token when needed:

```kotlin
val token = sharedPreferences.getString("jwt_token", null)
```

## Conclusion

In this lesson, we covered how to consume RESTful APIs in Android applications, handle JSON data using serialization and deserialization with Retrofit, and implement user authentication using OAuth and JWT. We also discussed secure storage of credentials using EncryptedSharedPreferences. Understanding these concepts will enable you to build secure applications that interact with web services effectively. In the next lesson, we will explore advanced topics in Android development, including testing and debugging best practices.