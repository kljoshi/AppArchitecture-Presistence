 # App Architecture-Presistence 

This is the Sleep tracker app for lesson 6 of the Udacity: Developing Android Apps with Kotlin Course. 

- How to store data 
- Introduction to room data base
- Using Data Access Object (DAO)
- Database queries
- Coroutines - How to make sure your long running database operations don’t slow down the app for the user by using Kotlin coroutines. 
- Use of safeArgs. 

### Introduction
In most applications, you have some data that you want to hold onto, even when the user closes and leaves the app. 

For example, you might want to store a playlist or an inventory of game items, records of expenses and income, a catalog of constellations, or sleep data over time. 

In this lesson we are going to build a sleep quality tracker app and use a database to store the sleep data over time. 
<img width="252" alt="Ul Controller" src="https://user-images.githubusercontent.com/43662326/147375715-9bf2a132-3e9b-45a4-9372-8cc7d64d109d.png">

This app uses a simplified architecture with a UI controller, ViewModel and LiveData, and a room database. 

----
### What is roommate base? 

Room is a database library that is part of Android jet pack. Room takes care of many of the chores of setting up and configuring a database. And It makes it possible for your app to interact with a database using ordinary function calls. 

----
### Designing Entities

The first thing we need is data, which in Kotlin and Android, we represent in data classes, and we need a way to act on that data and our database, which in Kotlin would be function call. 

In Room, we need entities and queries. 

#### Entity

Entity represents object or concept to store in the database. Entity class defines a table, each instance is stored as a table row. 

Note: Entity as data class. 

#### Query 

Query is a request of data or information from a database table, or a combination of tables, or a request to perform some action on the data. Ex: inserting or deleting data.   Note: Query as interface. 

----

<img width="280" alt="Ul Controller" src="https://user-images.githubusercontent.com/43662326/147375727-2d63828f-4e8d-4a1e-8b6f-c17668c10601.png">

We use data classes to define our tables and we use annotations to specify things such as which property represents the primary key. We create interfaces that define how to interact with the database which is by DAO.

----

### How to create Entity
1. Create the data class with parameters. Example: ID, start time and end time. 
2. Annotate the data class with @Entity, and name the table.
```@Entity(tableName = "daily_sleep_quality_table") ```
3. Identify the primary key by annotating it with @PrimaryKey.
```@PrimaryKey(autoGenerate = true)```
4. Annotate the remaining properties with @ColumnInfo.
```
@Entity(tableName = "daily_sleep_quality_table")
 data class SleepNight(
  @PrimaryKey(autoGenerate = true)
  var nightId: Long = 0L,

  @ColumnInfo(name = "start_time_milli")
  val startTimeMilli: Long = System.currentTimeMillis(),

  @ColumnInfo(name = "end_time_milli")
  var endTimeMilli: Long = startTimeMilli,

  @ColumnInfo(name = "quality_rating")
  var sleepQuality: Int = -1)
```
----

### how to create Data Access Object (DAO)
When using Room DB, we query the database by defining and calling Kotlin functions in our code that map to SQL queries. We define those mappings in a DAO using annotation and Room creates the necessary code for us. 

DAO Annotations
- @Insert
- @Delete
- @Update
- @Query

Steps to create DAO:
1. Create an interface and annotate it with @Dao.
2. Add an @Insert function.
3. Add @Query function

Note: we can return LiveData in room DB. This is one of the amazing features of room. Room makes sure that this live data is updated whenever the database is updated. This means that we only have to get this list once. Attach an observer to it and then if the data changes, the UI will update itself to show the changed data without use having to get the data again.   This saves time, code complexity and most likely a couple of debugging hours on top. 

----

### Creating room database 
Now that we finally have an entity and a DAO, we can move forward with the database. We need to create an abstract database holder class annotated with database. 

This class has method that either creates an instance of the database, if it doesn’t exist or returns a reference to an existing database. 

Steps to creating a room database:
1. Create a public abstract class that extends Room database. 
2. We annotate the class with database in the arguments, declare the entities for the database and set the version number. 
3. Inside a companion object, we define an abstract method or property that returns database DAO. 
4. We only need one instance of the same Room database for the whole app, so we make the Room database a singleton. 
Note: We use Room’s database builder to create the database only if it doesn’t exist. Otherwise, we return the existing database. 

Note: Meaning of @Volatile annotation - This helps us to make sure the value of Instance is always up to data and the same to all execution threads. The value of a volatile variable will never be cached, and all writes and reads will be done to and from the main memory, it means that changes made by one thread to instance are visible to all other threads immediately.  And we don’t get the situation where, two threads each update the same entity in a cache. 

----

### Abstract keyword: 

Abstract keyword is used to declare abstract classes in Kotlin. And abstract class cannot be instantiated (you cannot create objects of an abstract class). However, you can inherit subclasses from them.

----

### Companion object

Companion object are singleton objects whose properties and functions are tied to a class but not the the instance of that class. 

----

### Adding ViewModel 

https://classroom.udacity.com/courses/ud9012/lessons/fcd3f9aa-3632-4713-a299-ea39939d6fd7/concepts/15877d76-9040-40d6-8978-a8209fa6f627

----

### Multithreading and Coroutines

https://classroom.udacity.com/courses/ud9012/lessons/fcd3f9aa-3632-4713-a299-ea39939d6fd7/concepts/f8c76b29-9f8e-4402-9ff9-d1ec4e3f9312 

#### Multithreading Getting data from the database might take a long time if there are a lot of data. This long running operation should run on a operate thread. 
 The operating system can enable an application to create more than one tread of execution within a process. This is called multithreading. 

On android, the main thread is a single thread that handles all updates on the UI. The main thread is also the thread that calls all click handlers and other UI and life cycle callbacks. This is why it’s also called the UI thread.  The UI thread is the default thread, meaning unless you explicitly switched threads or use a class that runs on a different thread, everything you do is on the main thread. 

We should never perform long-running operations on the main or UI thread. Because calling code like this from the main thread can cause the app to pause, stutter or even freeze. If the main thread is blocked for too long, the app may even crash and present an application not responding dialogue. 

#### Coroutines

A pattern for performing long-running tasks without blocking the main thread is callbacks. By using callbacks, you can start long running tasks on a background thread.    When a task completes, the callback supplied as an argument is called to inform you of the result on the main thread. 

In Kotlin, we have coroutines to handle long-running tasks elegantly and efficiently. Kotlin coroutines let you convert callback-based code to sequential code. Code written sequentially is typically easier to read and can even use language features such as exceptions. 

In the end, coroutines and call back do exactly the same thing, wait until a result is available from a long-running task and continue execution. 

Characteristics of Coroutines:
- They are asynchronous
- Non-blocking
- And use suspend function to make asynchronous code sequential.

#### What is asynchronous?
Asynchronous means that a coroutine runs independently from the main execution steps of your program. This could be in parallel, on a separate processor, but could also be that while the rest of the app is, for example, waiting for input.

One of the important aspects of async is that we cannot expect the result is available to us until we explicitly wait for it. For example, let’s say you have a question that requires some research and you ask a colleague to find you the answer, they go off and work on it, which is asynchronously and on a separate thread.  Unless you wait for the answer, you can continue to do other work that does not depend on their answer, until they come back and tell you what the answer is. 

#### Non-blocking 
Non-blocking means the system is not blocking the main or UI thread. So users will always have the smoothest possible experience, because the UI interaction will always have priority. 

Note: Because our coroutine code is compiled from sequential code, we don’t need to specify callbacks. And for coroutines, the compiler will make sure the results of the coroutines are available before continuing or resuming. 

#### suspend keyword

The keyword suspend is Kotlin’s way of marking a function or a function type available to coroutines. When a coroutine calls a function marked for suspend,  instead of blocking until that function returns like a normal function call, it suspends execution until the result is ready, then it resumes where it left off with the result. Now, while it’s suspended, waiting for a result, it unlocks the threads that its running on, so other functions or coroutines can run. 

<img width="291" alt="Blocked thread" src="https://user-images.githubusercontent.com/43662326/147375878-e53a8374-9b49-49b2-8636-751f80de02a4.png">

So the difference between blocking and suspending is that if a thread is blocked, no other work happens, if the thread is suspended, other work happens until the result is available. 

Be aware that the suspend keyword does not specify this thread code runs on. Suspend functions may run on a background thread or a main thread. 

#### Coroutines need…
- A job - A job is anything that can be canceled. Jobs can be arranged into parent-child hierarchies so that cancellation of a parent leads to an immediate cancellation of all its children, which is a lot more convenient that if we had to do this manually for each coroutine.   
- A dispatcher - the dispatcher sends off coroutines to run on various threads. For example, dispatcher.main will run task on the main thread, and dispatcher.IO is for offloading blocking IO tasks to a shared pool of threads.  
- A scope - the scope combines information, including a job and dispatcher, to define the context in which the coroutine runs. Scopes, keep track of coroutines. When you launch a coroutine, it’s in scope, which means that you’ve said which scope will keep track of the coroutine. 
----

### Coroutines for Long-running Operations

https://classroom.udacity.com/courses/ud9012/lessons/fcd3f9aa-3632-4713-a299-ea39939d6fd7/concepts/7e5d7478-eca3-466c-bc1b-7997dcab696d 

For more information look at the comment section of SleepTrackerViewModel.kt to learn about coroutines.

----

### Recap

- How to use Room database
- Created SleepNight data class
- Created Data Access Object 
- Room database singleton
- ViewModelFactory for dependency injection
- ViewModels
- Coroutines
- Observable state variables.
