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
```@Entity(tableName = "daily_sleep_quality_table")data class SleepNight(```
```@PrimaryKey(autoGenerate = true)```
```var nightId: Long = 0L,```
```@ColumnInfo(name = "start_time_milli")```
```val startTimeMilli: Long = System.currentTimeMillis(),```
```@ColumnInfo(name = "end_time_milli")```
```var endTimeMilli: Long = startTimeMilli,```
```@ColumnInfo(name = "quality_rating")```
```var sleepQuality: Int = -1)```





## SleepQualityTracker

The SleepQualityTracker app is a demo app that helps you collect information about your sleep. 
* Start time
* End time
* Quality
* Time slept

This app demonstrates the following views and techniques:
* Room database
* DAO
* Coroutines

It also uses and builds on the following techniques from previous lessons:
* Transformation map
* Data Binding in XML files
* ViewModel Factory
* Using Backing Properties to protect MutableLiveData
* Observable state LiveData variables to trigger navigation

## Screenshots

![Screenshot1](screenshots/sleep_quality_tracker_start.png)
![Screenshot2](screenshots/sleep_quality_tracker_stop.png)
![Screenshot3](screenshots/sleep_quality_tracker_quality.png)

## How to use this repo while taking the course


Each code repository in this class has a chain of commits that looks like this:

![listofcommits](https://d17h27t6h515a5.cloudfront.net/topher/2017/March/58befe2e_listofcommits/listofcommits.png)

These commits show every step you'll take to create the app. Each commit contains instructions for completing the that step.

Each commit also has a **branch** associated with it of the same name as the commit message, as seen below:

![branches](https://d17h27t6h515a5.cloudfront.net/topher/2017/April/590390fe_branches-ud855/branches-ud855.png
)
Access all branches from this tab.

![listofbranches](https://d17h27t6h515a5.cloudfront.net/topher/2017/March/58befe76_listofbranches/listofbranches.png
)


![branchesdropdown](https://d17h27t6h515a5.cloudfront.net/topher/2017/April/590391a3_branches-dropdown-ud855/branches-dropdown-ud855.png
)

The branches are also accessible from the drop-down in the "Code" tab.


## Working with the Course Code

Here are the basic steps for working with and completing exercises in the repo.

The basic steps are:

1. Clone the repo.
2. Check out the branch corresponding to the step you want to attempt.
3. Find and complete the TODOs.
4. Optionally commit your code changes.
5. Compare your code with the solution.
6. Repeat steps 2-5 until you've gone trough all the steps to complete the toy app.


**Step 1: Clone the repo**

As you go through the course, you'll be instructed to clone the different exercise repositories, so you don't need to set these up now. You can clone a repository from github in a folder of your choice with the command:

```bash
git clone https://github.com/udacity/REPOSITORY_NAME.git
```

**Step 2: Check out the step branch**

As you go through different steps in the code, you'll be told which step you're on, as well as a link to the corresponding branch.

You'll want to check out the branch associated with that step. The command to check out a branch would be:

```bash
git checkout BRANCH_NAME
```

**Step 3: Find and complete the TODOs**

Once you've checked out the branch, you'll have the code in the exact state you need. You'll even have TODOs, which are special comments that tell you all the steps you need to complete the exercise. You can easily navigate to all the TODOs using Android Studio's TODO tool. To open the TODO tool, click the button at the bottom of the screen that says TODO. This will display a list of all comments with TODO in the project. 

We've numbered the TODO steps so you can do them in order:
![todos](https://d17h27t6h515a5.cloudfront.net/topher/2017/March/58bf00e7_todos/todos.png
)

**Step 4: Commit your code changes**

After You've completed the TODOs, you can optionally commit your changes. This will allow you to see the code you wrote whenever you return to the branch. The following git code will add and save **all** your changes.

```bash
git add .
git commit -m "Your commit message"
```

**Step 5: Compare with the solution**

Most exercises will have a list of steps for you to check off in the classroom. Once you've checked these off, you'll see a pop up window with a link to the solution code. Note the **Diff** link:

![solutionwindow](https://d17h27t6h515a5.cloudfront.net/topher/2017/March/58bf00f9_solutionwindow/solutionwindow.png
)

The **Diff** link will take you to a Github diff as seen below:
![diff](https://d17h27t6h515a5.cloudfront.net/topher/2017/March/58bf0108_diffsceenshot/diffsceenshot.png
)

All of the code that was added in the solution is in green, and the removed code (which will usually be the TODO comments) is in red. 

You can also compare your code locally with the branch of the following step.

## Report Issues
Notice any issues with a repository? Please file a github issue in the repository.


