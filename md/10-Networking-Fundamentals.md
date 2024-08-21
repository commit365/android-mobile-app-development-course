# Lesson 10: Networking Fundamentals

In this lesson, we will explore the fundamentals of networking in Android applications. We will learn how to make network requests using Retrofit and OkHttp, handle errors, and implement response caching. Additionally, we will discuss RESTful APIs, how to handle API responses, and implement pagination and search features.

## 1. Making Network Requests Using Retrofit and OkHttp

### What is Retrofit?

Retrofit is a type-safe HTTP client for Android and Java that simplifies the process of making network requests. It allows you to define your API endpoints using annotations and handles the conversion of JSON responses into Kotlin objects.

### Setting Up Retrofit

1. **Add Dependencies**

To use Retrofit, add the following dependencies in your `build.gradle` file:

```groovy
dependencies {
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0' // For JSON conversion
    implementation 'com.squareup.okhttp3:logging-interceptor:4.9.2' // For logging
}
```

2. **Create a Data Model**

Define the data model that matches the structure of the JSON response from the API:

```kotlin
data class User(
    val id: Int,
    val name: String,
    val email: String
)
```

3. **Define the API Interface**

Create an interface that defines the API endpoints using Retrofit annotations:

```kotlin
import retrofit2.Call
import retrofit2.http.GET
import retrofit2.http.Path

interface ApiService {
    @GET("users/{id}")
    fun getUser(@Path("id") userId: Int): Call<User>

    @GET("users")
    fun getUsers(): Call<List<User>>
}
```

4. **Create the Retrofit Instance**

Set up the Retrofit instance in your application:

```kotlin
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

object RetrofitClient {
    private const val BASE_URL = "https://jsonplaceholder.typicode.com/"

    val instance: ApiService by lazy {
        val retrofit = Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
        retrofit.create(ApiService::class.java)
    }
}
```

### Making Network Requests

To make a network request, call the appropriate method from the `ApiService` interface:

```kotlin
RetrofitClient.instance.getUser(1).enqueue(object : Callback<User> {
    override fun onResponse(call: Call<User>, response: Response<User>) {
        if (response.isSuccessful) {
            val user = response.body()
            // Handle the user object
        } else {
            // Handle the error
        }
    }

    override fun onFailure(call: Call<User>, t: Throwable) {
        // Handle failure
    }
})
```

## 2. Error Handling and Response Caching

### Error Handling

When making network requests, it’s essential to handle potential errors gracefully. Here’s how to manage errors:

- **Check Response Codes**: Always check the response code to determine if the request was successful.

```kotlin
if (response.isSuccessful) {
    // Handle successful response
} else {
    // Handle error response
    when (response.code()) {
        404 -> Log.e("Error", "User not found")
        500 -> Log.e("Error", "Server error")
        else -> Log.e("Error", "Unknown error")
    }
}
```

- **Handle Network Failures**: Use the `onFailure` method to handle network failures, such as no internet connection.

### Response Caching

To enable response caching with OkHttp, configure an OkHttpClient with a cache:

1. **Create a Cache**

```kotlin
val cacheSize = 10 * 1024 * 1024 // 10 MB
val cache = Cache(context.cacheDir, cacheSize.toLong())
```

2. **Set Up OkHttpClient**

```kotlin
val okHttpClient = OkHttpClient.Builder()
    .cache(cache)
    .addInterceptor(loggingInterceptor) // Optional: for logging
    .build()
```

3. **Use OkHttpClient with Retrofit**

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl(BASE_URL)
    .client(okHttpClient) // Set the custom OkHttpClient
    .addConverterFactory(GsonConverterFactory.create())
    .build()
```

## 3. Understanding RESTful APIs

### What is a RESTful API?

A RESTful API (Representational State Transfer) is a web service that follows REST principles. It uses standard HTTP methods (GET, POST, PUT, DELETE) to perform operations on resources, which are typically represented in JSON format.

### Common HTTP Methods

- **GET**: Retrieve data from the server.
- **POST**: Send data to the server to create a new resource.
- **PUT**: Update an existing resource on the server.
- **DELETE**: Remove a resource from the server.

### Handling API Responses

When you make a request to a RESTful API, you will typically receive a JSON response. Use Retrofit’s built-in Gson converter to automatically convert the JSON response into Kotlin objects.

### Example of Handling a Response

```kotlin
RetrofitClient.instance.getUsers().enqueue(object : Callback<List<User>> {
    override fun onResponse(call: Call<List<User>>, response: Response<List<User>>) {
        if (response.isSuccessful) {
            val users = response.body() // List of users
            // Handle the list of users
        }
    }

    override fun onFailure(call: Call<List<User>>, t: Throwable) {
        // Handle failure
    }
})
```

## 4. Implementing Pagination and Search Features

### Pagination

Pagination is essential for loading large datasets efficiently. You can implement pagination by passing parameters to your API requests.

1. **Modify API Endpoint for Pagination**

Assume your API supports pagination through query parameters:

```kotlin
@GET("users")
fun getUsers(@Query("page") page: Int, @Query("limit") limit: Int): Call<List<User>>
```

2. **Load More Data**

When the user scrolls to the bottom of the list, load more data:

```kotlin
fun loadMoreUsers(page: Int) {
    RetrofitClient.instance.getUsers(page, 10).enqueue(object : Callback<List<User>> {
        override fun onResponse(call: Call<List<User>>, response: Response<List<User>>) {
            if (response.isSuccessful) {
                val newUsers = response.body()
                // Add new users to the existing list and update the adapter
            }
        }

        override fun onFailure(call: Call<List<User>>, t: Throwable) {
            // Handle failure
        }
    })
}
```

### Search Features

To implement search functionality, you can modify your API to accept a search query:

1. **Modify API Endpoint for Search**

```kotlin
@GET("users")
fun searchUsers(@Query("query") query: String): Call<List<User>>
```

2. **Call the Search API**

When the user submits a search query, call the search API:

```kotlin
fun searchUsers(query: String) {
    RetrofitClient.instance.searchUsers(query).enqueue(object : Callback<List<User>> {
        override fun onResponse(call: Call<List<User>>, response: Response<List<User>>) {
            if (response.isSuccessful) {
                val searchResults = response.body()
                // Update the RecyclerView with search results
            }
        }

        override fun onFailure(call: Call<List<User>>, t: Throwable) {
            // Handle failure
        }
    })
}
```

## Conclusion

In this lesson, we covered the fundamentals of networking in Android applications using Retrofit and OkHttp. We learned how to make network requests, handle errors, and implement response caching. We also discussed RESTful APIs, how to handle API responses, and how to implement pagination and search features. Understanding these concepts will enable you to build robust applications that interact with web services effectively. In the next lesson, we will dive into working with local databases using Room for data persistence.