
01 - Introduction to Lesson 4
=============================

>> The goal is to ensure your app provides a seamless, efficient experience for
your users, even when networks may be slow or unavailable


02 - Why We Need an Activity Lifecycle
======================================

Okay.
So before Dan gets carried away with databases, let's look at
what happens when you use an intent to transition between activities.
Within Sunshine,
if you tap on a list in the main activity it opens up the detail activity.
But the main activity remains on the backstack.
Ready to reappear as soon as you hit Back.
Now, in that example, both activities are within the same app.
But as we learned in lesson three, you can also watch the browser or
maps app from sunshine.
Or for that matter, the user can hit Home.
And launch apps from there, or they can use the recent key or
switch to another app using notifications.
All of these options mean you can end up with a lot of apps on your back stack.
Now, keep in mind that our resources on devices are extremely limited.
So it's not a good idea to have dozens of apps sitting idle in the background.
Android solves this for us,
so you don't have to use those awful task killer apps.
So, how does it do that?
Well, it kills low priority applications that you haven't used in a while.
We'll go into detail on exactly how it figures out which app needs to die,
in lesson six.
But for now, it's important to realize that your app isn't in
control of its own destiny.
It can be killed at any time.
So you need to know, how to deal with that.
And that means understanding the lifecycle of an activity and the signals we
get from the framework to indicate its progress through the lifetime.


03 - The Android Activity Lifecycle
===================================

A platform which terminates apps on its own is a pretty radical departure for
anyone like me who cut their teeth on desktop Winforms development.
If you come from a similar background you probably have
a pretty good idea of what to expect in terms of lifecycle event handles.
You start with the onCreate call back where you build and wire up your UI.
Once that's done, your activity has been created. It likely won't be a surprise
to you that there are also event handlers for when the activity becomes visible.
Which is onStart and another for when it gets focus and becomes the active
foreground app, which is onResume. That same sequence, then happens in reverse.
[SOUND] onPause indicates that the activity has lost focus, followed by
onStop when the app is no longer visible. Finally, there's an onDestroy method,
indicating the end of the app lifecycle. When your app is first launched, you'll
quickly move through these states. Create, Start, Resume, one after the other
[BLANK_AUDIO]
But within the full application lifetime, from when onCreate is first called and
till the app is finally terminated. It will move through the active lifetime and
the physical lifetime, many times. Let's look at those in a little more detail.


04 - Active and Visible Lifetimes
=================================

The active life cycle is when your activity is in the foreground and
has focus. Here it is actively receiving input from user events and
no other activities are obscuring it. On [UNKNOWN] and
the active lifetime ends as soon as your activity is partially obscured,
like in this example. [SOUND] You can see here the permissions dialogue
is displayed in front of Google Play for
an app in store. Or the same thing happens when you have another activity trying
to fulfill an implicit intent and the user needs to make a selection. So
to make efficient use of limited resources,
you'll want to use these signals to adjust your app's resource burden.
So, most updates through a UI, can be paused when this lifetime ends,
which is announced by onPause. But as you see, the app is still visible, so
you shouldn't pause any processes that are drawing your UI. The visible
lifetime on the other hand continues whenever the app is at all visible and
ends as soon as it's completely obscured by another app. Like this.
[SOUND] At this point, our app is moved to the background.
So when you see on stop, you know the user can't see your app at all. So, while
OnCreate and OnDestroy will be called at most once, each time your app is run,
these handlers are likely to be called many times, while the app is running.
Now, this is where things get a little different. On almost al platforms,
app life cycles are deterministic. Generally, you'll start a program and
it'll keep running, until it either completes or the user cancels it. You look
at traditional desktop development that means your app keeps running until your
user chooses quit or exit from the file menu. At that point you can say stay and
free resources. But as we know on Android life cycles work a little differently.
So, let's take a closer look at exactly how that works now.


05 - Lifecycle Events
=====================

So now go ahead an add logging statement to the onpause, onresume, onstop,
onstart and oncreate and ondestroy life cycle handlers for the main activity.
Start the app and try rotating your phone. Now observe the logs.
Whats the sequence of these lifecycle events that your activity goes through,
simply by changing its orientation.


06 - Lifecycle Events Solution
==============================

Starting with tear down, the app will first be paused, stopped, destroyed and
then recreated, restarted and resumed.


07 - Activity Termination
=========================

Let's see if we can figure out what happens when an app is terminated in
the background. Start the app again then hit the Home key. Now launch some other
apps before returning to ours. Try and pick apps with large memory footprints.
Can you figure out, which of the following lifecycle event handlers is the last
one guaranteed to be called, before your app maybe terminated by the runtime?


08 - Activity Termination Solution
==================================

Since Honeycomb, Android apps can rely on, on stop being called, before the app
is at risk of being terminated. But if you're supporting pre-Honeycomb devices,
you should prepare for termination, as early as after on paused.
[BLANK_AUDIO]
Let's take a closer look at how you should prepare for an untimely app death.


09 - How to Prepare for Termination
===================================

As you are moving around between apps, you probably notice that from a user
perspective, Android doesn't announce changes in app state. It doesn't announce,
that it's low in memory or ask users to close apps, to free up resources. In
fact, it does everything it can, to make the resource limitation to the device,
invisible to the user. That means keeping the foreground app running smoothly,
and switching between apps seamless, and that means killing apps in
the background. And it does that if it
needs their resources to make sure that the foreground apps can continue to run.
So we know that as soon as your app isn't visible, its lifetime is as
perilous as that of a Stark at the Red Wedding. Likely to die without notice,
but ready to return from the dead. That tells us some very important things
about how our app should behave to be good citizens and provide a great user
experience. From a system perspective, on pause and on stop are signals that our
app may be killed eminently. So we need to clean up any resources that
need an orderly tear down such as closing any open connections or sockets.


10 - What To Do in OnPauseOnStop
================================

It's also a signal to app that the system now considers it alert priority. Given
the limited resources available, we should respond by reducing our use of system
resources. Particularly, anything that could continue draining the battery.
That includes anything that's being used exclusively to update the UI. So, what
are some examples of listeners or updates that should be paused or disconnected
when onpause or onstop are received? Put your answer in the text box below.


11 - What To Do in OnPauseOnStop Solution
=========================================

Some good examples of answers include,
sensor listeners, location updates, dynamic broadcast receivers. Or
games physics engines. Any task that your application needs to keep happening,
even when your activity is paused. Shouldn't really happen within an activity at
all. We'll have a look at some solutions to that problem in lesson six.
Keep in mind that when you're activity is paused,
it's still visible. It's just obscured by something else, usually a dialog box.
But it's not stopped. So, don't stop drawing your UI when you receive on pause


12 - Activity Lifecycle Recap
=============================

For the purposes of the Sunshine app, this information is purely theoretical.
In fact, you'll really only need to consider this later,
when you're adding things like Sensor or LocationListeners. Until then,
the default components will handle much of this behavior for
you. In any case, now that you understand the lifecycle and
the way the system handles exiting your app, when it needs its resources.
You should hopefully understand why Exit or Quit buttons in Android apps serve
no practical purpose. At most, you can call finish on an activity and
it will get torn down instantly. That's actually exactly what happens when
you use a simple hits back from within their activity.
If you're still not convinced, check out the video in the instructor notes.
Where I explain my reasoning with a little more, color.


13 - Maintaining State
======================

As app developers, it's our job to maintain the illusion that once started,
every app is waiting patiently in the background looking for
its chance to be the star when called on. So whenever the user switches back to
your app, whether or not the system is kilted in the interim,
they should be presented with the same UI they had when they left. To help,
Android has a pair of handles specifically for persisting state in these
circumstances. On save instance data is called immediately before on pause. So
as soon as your app is no longer active, an on
restore instance data is called immediately after on create, but
only if the app is being restarted after having been terminated by the system.
That means you can read the bundle of state information saved the last time your
app was moved to the foreground here. The next time,
you'll use the switches to your app even if it was killed by the system in
the mean time. Using that bundle you can return your ui to the same state it
was the last time the user saw it creating a seamless transition that hides
the resource management happening under the covers. That's enough of the why,
let's go back to Dan and see how to put this theory into practice.


14 - Bundles to Save App State
==============================

You may be thinking, lifecycles are really important.
However this section, is on persisting and recovering data. Trust me,
I just want to get us coding again. However, these things are closely linked and
they're easy to get wrong if you don't understand both. As Reito mentioned,
Android saves the state of the application in bundles. You might have had
the idea that you would just save all sorts of information in these bundles and
not have to worry about any other form of persistence. But, the thing is,
these bundles go away as soon as the user hits the Back key in
your main activity. It's really important that they do this.
When the user chooses to close your activity with the Back key, the expectation
is that next time your activity is displayed, it will be in the default state.
If the user backgrounds your app using the Home key or the App Switcher,
the next time your activity is created, it should resume from the current state.


15 - Good Android Citizen
=========================

Another thing that I wanted to mention before we dive into the code is that it's
important to be a good Android citizen. This means adhering to the principle of
minimizing network activity and having an app that works seamlessly between
offline and online states. We're going to make that happen by creating a cache
data model that is shared between the various activities of Sunshine.


16 - Storing Data in Android
============================

Before we dive into the session material, let's talk a bit about storing or
persisting data in Android. First of all, why do we bother to persist things
at all? This is the era of the connected cloud. Why don't we always fetch from
there? It's really nice to not start the app and see, Loading, or, even worse,
like this, a blank screen. The faster people can use an app, the more it will be
used. Obviously, if one has to select Refresh, to get it to display anything,
that would be particularly bad. Another reason to persist our data,
is that using any radio is detrimental to the battery life of the device,
especially the cellular radio. Many users aren't on meter data plans or
may be roaming when they want to use your app. All those unnecessary data
fetches can add up. There are still lots of places that don't have a network
connection available. One of the prime advantages of having a mobile app is
being resistant to bad or non-existent network conditions. After all,
you never know where the user will want to use your app. As you might expect,
Android stores your persistent data in the file system.
These files can be stored in internal storage that is private to your app.
They can also be stored in shared or external storage. On older
Android devices this shared storage was actually on an external memory card.
Today, most Android devices only emulate this card so that there is
the shared external storage apps need available on the device. Some Android
devices have emulated shared storage and secondary external storage.
Android 4.4 Kit Kat added an API to allow developers to access this secondary
external storage. We're going to focus on internal storage in this class.
Check the instructor notes to learn more about Android storage locations.
As I mentioned before, Android persists data into the file system.
It does provide two functional layers on top of the file system in the form of
shared preferences and SQLite. The Shared Preferences class provides a general
framework that allows you to save and retrieve persistent key value pairs of
primitive data types, such as booleans, floats, ints, longs and
strings. Shared Preferences is used by the Android preference activity to store
our settings data such as the location. Why store things in a SQLite database?
After all, Android supports both RAW files and Shared Preferences. For
the same reason that it's inefficient to find things if you throw your
clothes in a pile on the floor. Storing things in an SQLite database helps
you organize and find data easily, thanks to the power of indexing in tables.
An SQLite database looks something like this fragment from our weather database.
Note that not all fields are represented. We can perform queries using
SQL against this database, such as the SELECT statement here, which returns
the weather on the specified date, similar to what we'll want to do for
the Detail view. We can use a slightly more complex query to return a range of
dates, which is similar to what we do on the main forecast ListView.


17 - SQLite Databases
=====================

Okay. Here's a slightly more complicated SQL query. See the instructor notes for
some resources that you can use to help parse the SQL statement. One note,
max is the name of the column that stores the high temperature for the day.


18 - SQLite Databases - Solution
================================

It will actually return a single row containing the max high temperature.
We're using a descending order for this column, which will place the largest
value at the beginning of the query. The limit statement tells SQLite to
only return a single row. We return a row that has the largest value.
If we wanted to get the most recent high, we could order by max desc, date desc.


19 - More on Storing Data in Android
====================================

Using SQLI makes it easy to delete, update, and insert rows using highly
expressive queries. One can even add new columns as you revision your database.
It's not a surprise that just about popular app makes some use of SQLI to
Android. It's a very popular and important component of the Android solution.
Android also provides a huge set of helper functions to make working with
databases easy, including functions to automate query building.
Display ListView is populated by queries and even compile statements. For
more information about databases in Android, see the instructor nodes. One thing
that I wanted to mention is that we're teaching you the fundamentals of how to
build database in a content provider on Android. The idea is that once you
are done with this class, you'll have a good understanding of how it all works,
even if you end up using a library to help build your database and
content provider in your future applications. That's it for
a brief intro into storage on Android. If you want to learn more,
go to developer.android.com and search on Data Storage.


20 - Final Detail Wireframe
===========================

This is a good time to revisit our final detailed wire frame.
For each weather forecast, we're going to need to store at least the date,
the condition, the high and low temperature,
the humidity. The wind speed and direction, and the barometric pressure.
Since our database will use row to represent each forecast day, we'll store
each piece of data associated with the forecast in a separate column.


21 - WeatherContract
====================

Let's start by defining a database contract between our data and
our model. A contract is an agreement between our data model and our
views describing how information is stored. It will contain all the fields that
our user interface will display. Let's go into Android Studio to begin coding.
We'll add a new package to our project named data to encapsulate the data model.
Next, we'll create a contract class to store our column information.
The inner classes within our contract class will be used to define tables.
Each table will implement base columns because the columns represented by base
columns are useful. The ID column, in particular, must be part of our table
in order for our content provider integration to work later on in this lesson


22 - Our First Table
====================

Our database will contain weather forecast entries. Out data model will use
two tables. One table will be used to contain information about the location,
while the other will contain the forecast data keyed the locations.
These will ultimately be tied back to our view through the contract, and
the content provider. We can use an inner join, pull the complete data for
each forecast today. Including all information about the location. This,
is a big contract. Note that we store the location id, which will be a foreign
key from the location table in COLUMN_LOC_KEY. Note that the units aren't stored
in the database. We expect all weather entries to be stored in metric units, and
converted when needed by the UI, into imperial units. Since the column names
don't actually contain data types, it's useful to annotate that in
the variable names and/or the comments to make our contract more explicit.


23 - Columns
============

We need to create a contract for our other table that contains location entries.
Location entries must contain a city name. This innerclass will be
created in much the same way as weatherentry. It will implement base columns.
Contain the table name and contain the names of each column. Note: the idea of
each row in the location entry class, will be stored in WeatherEntry.


24 - Columns Solution
=====================

All right, you're done! Let's go take a look at my solution. Now note,
you won't be able to actually run this code to test any of these solutions yet.
But don't worry, we'll be testing soon. Here's our solution.
We begin with our unique table name. The rest of the inner class,
lists the columns we're going to use to store our data.


25 - Create Database with SQLiteOpenHelper
==========================================

Now that's a contract. But we still don't have a database.
Our database class will extend an Android class. SQLITEOpenHELPER.
SQLITEOpenHELPER contains cool stuff to help us handle database versioning.
As we make changes to our database in the future, it will help us modify our
tables. For many apps, being able to upgrade to a new version without data loss
is critical. Let's create a WeatherDBHelper class within the data package and
have that extend SQLite Open Helper. [SOUND] We can add
the required methods by hitting Ctrl+I.
And we can override the constructor by hitting Ctrl+O. So now we can hard code
these variables to constructor. Hard coding name to database name,
our factory to null, and our version to database version.
Now, you'll note I have made the database name public, and
that's because we're going to use it in our tests in the future.
Now you see we've got it on create and an onUpgrade method.
In the OnCreate method, we're going to start by creating a string to build
the weather entry table using data defined within the weather entry contract.
Now I'm just going to add this comment, so you know where to go back and
add the location entry stuff later. Since weather entry depends on
location entry, I would normally write location entry first, but
weather entry is pretty complicated. So I'd rather explain what we've done and
leave location entry up to you. We're going to use raw SQL for
our create table query, beginning with the table name from our contract.
At this point, it's helpful to import WeatherContract.locationentry and
weather entry. It makes our query so much easier to read. We'll start with
our ID field, which we'll set as our primary key in an auto increment field.
Note that integer is actually a signed value up to eight bytes long in SQLite.
Using the auto increment feature doesn't do precisely what one might think.
Setting up the ID is an integer primary key actually makes
the value unique whenever you do an insert, but it may not always increment.
It may reuse existing ID values of the records have been deleted.
What autoincrement is really useful for, is if you're synchronizing data two
ways with the server. But, we'll take advantage of it here because it makes data
from our queries sort a bit more naturally because we insert them in the right
order coming from the server. In general, we're using constraints on fields.
In this case, not null. We do this because it allows the database to do much of
our parameter validation for us. The tricky part about doing it this way,
is that we don't get useful errors when these constraints fail, so
it can be challenging to debug. We're using a human readable string for
the date. There's no strong reason for or
against this choice. We wanted to normalize the date to simplify our queries,
and human readability simplifies debugging. The date comes from open weather in
Unix time stamp format with some time information that we need to get rid of.
I'm not going to cover every field. Real means floating point in SQL.
It would have been fine to have used integer with fixed point math as well and
some would argue that this would be faster, but
real is more straight forward since we're storing floating point values. Now for
the fun stuff, we're going to set up a loc key.
It's a foreign key to the location entry table that you will be building.
This causes SQLite to enforce the relationship between the tables.
We cannot insert a weather entry when there is no corresponding location entry.
And we cannot delete a location entry if there are still weather entries that
depend on it. Cool stuff, right? One last constraint.
Our date text plus location must be unique. On conflict, replace the data.
This allows us to insert new data from the open weather EPI easily.
Preserving existing keys and updating the value as the forecast changes.


26 - LocationEntry
==================

Create a matching location entry table-creating query from the location entry
contract, using the weather entry one as a model. The good news is that this
is a simpler query. And if you really hate databases, just move to the solution.


27 - LocationEntry Solution
===========================

A much simpler table. Just a standard primary key, a few values, and
a few constraints. Note that we use ON CONFLICT IGNORE. This means in insert
into the database with the same key will not actually update the database at
all. And therefore will also not return an ID in the Android helper functions.


28 - SQLiteOpenHelper onUpgrade method
======================================

Well now we're getting somewhere. All we have to do is run those table creation
queries by calling exact sequel at the end of our on create function. But wait,
there's another function in our SQliteOpenHelper, onUpgrade. You might think,
why should I care about those?
I'm not upgrading anything yet. To make our development lives easier we're
going to implement the most standard kind of onUpgrade there is.
You see onUpgrade only fires if you change the version of your database.
Now remember, we passed that version into the SQliteOpenHelper base constructor,
ages ago. Since we are going to be using our database as a cache for web data
and not for user generated content, we'll drop the tables. This is helpful if we
want to change the database in the future. If we were using user data, we'd do
something like use alter table to add new columns. So, now we have a database.


29 - JUnit testing
==================

But it will be a while before we can test this in our code. We still are going
to implement a whole layer on top of it along with a bunch of UI changes.
Fortunately, we can implement J unit test here to help. And now for
some magic. We'll add a directory to the source directory of our Android studio
project called Android test. And then, another one underneath it called Java.
This is the way we tell Android studio there's a test target in cradle for
our app. Now we create a test package that matches our main package with
an extra test directory on the end. Create a new class called full
test suite in the test package. Add this code. This
is some boiler plate code that will allow us to add tests in additional classes.
Next, we'll create a TestDb class that extends Android test case [NOISE] and
add a test that creates our Db. The way this works is that the test runner
will execute every function in our class that begins with test in the order that
they are declared in the class. Each test should have a failure path that
uses an assert. We start by deleting the database before testing it. So,
we have a clean test. Now, to run the test we go to the app start drop down.
And select Edit Configurations. We select Plus to add a new configuration. And
select Android Tests Against Module App. Now we name the test.
Now note that it tends to target the emulator by default. So,
I want to use an actual device I'm going to select Show Chooser Dialogue.
Now we can just run the test against our device. And, it passes. So
let's create a data base insert and read test. At the beginning of it,
we'll insert a single record into each table. We'll begin with some dummy
data for our location. We'll use the dbHelper to get a writeable database.
This is exactly how we will use the database when we code it up in our project.
We'll then create a ContentValues object, which is a handy helper object that
Android uses to store values and keys. We'll store our dummy data into
the columns from our LocationEntry contract. Note that in order to make it
work with this abbreviated syntax, I did have to add some extra imports up here.
We then insert the data into our data base, and verified that we got a row back.
Now I find that it's helpful to put log messages into my test cases.
Now we'll use the database read operations to pull our dummy data back out of
the data base. We're making use of a custom projection here which, in theory,
would make it easy to use our database cursor to query for the values we want.
We use a custom projection here. Although, it's not required.
If the custom projection wasn't there, we would simply return all columns.
A database cursor is a control structure that enables traversal over
the records in a database. In Android, this is represented by a cursor object.
The cursor object allows one to traverse between records in a query and
get the contents of any individual column from a query.
Now, we use cursor.moveToFirst to populate our cursor with our row of data.
We can then pull out our data by index. Finally assert
if it doesn't match our dummy data. And now, we get to run our tests again.
[BLANK_AUDIO]
And they passed.


30 - InsertReadDbTest
=====================

All right, as the comment says below, now that we have a location let's add some
weather. Let's implement the weather part of our insert redb test. I'm going to
give you some dummy data to help you out. Note that the location key is the row
ID of the data we just inserted. Also note that I can use this short syntax for
weather entry, because I added this import at the top of the file.
Alright, so we insert that provided data that I gave you into our database, and
verify that we can read it back out again.


31 - InsertReadDbTest Solution
==============================

All right, you're done. I hope you don't mind that I gave you
the larger table this time. But it depends on the first one, so it would've been
hard to give you in any other order. I gave you the content value structure, so
it's pretty easy to just insert the data. Then, we query the data back out.
[SOUND] If move to first fails, the cursor is empty, and
the query failed. [LAUGH] Lots of cut and paste here. It is nice to fetch the dv
values using the helper for the data type, although its not strictly necessary,
since we know our test data is actually going to match our constrains. And
there is one more failure case we need to add here, if our query doesn't return
any rows. And that's it. All that just to get a database and
a way to test it. Now let's run the test again. Well, at
least we know all the basic stuff works, and just remember, without these tests,
you wouldn't have been able to run this code for a very, very long time.


32 - Simplify Tests
===================

All right. Now for a little bit of an open ended exercise.
How could we simplify our test code? Write up a solution. A hint here is
that we can take advantage of the read property of the content value structure.


33 - Simplify Tests
===================

Here's how I ended up simplifying things. I'd love to see great testing ideas.
So, if you're particular to what you've created, post your solution to
the forums. I'm starting off refactoring things a little bit. What I'm
going to do is I'm going to add a function to get the location content values.
This will be useful when we actually try to use these tests for
some other things later on. And there we are. A function that'll
return location content values. I'm also going to pull the city name out so
we can use that for some validation steps later on. And there we are. It's now
inside of the function. And then we're simply going to call that function to
get our location content values and we can delete all those lines of code.
Now, all of these columns, as I said before, are not very important, so
we can delete them. And we can also null out the value in our query.
The next thing I want to do is actually fix this validation step. What we can do
is we can create a function that relies on the fact we can get the map and
set from our content values. And then we can simply iterate through those.
Then we can see if the values that we used to create the record actually past
the values in the cursor that are returned. Now, back to our task. So,
there's a lot of unnecessary code here now. All we have to do is call
validateCursor with our values and our cursor. All right. So now we've
converted the first test. We can do the same thing with the second test as well.
First of all, let's do the same abstraction we did before. Now, of course,
one of these values isn't going to be static, so we have to add that in.
So now we can say, content values equals getWeatherContentValues.
With our all important location row ID. Remember, our tables are linked.
And then we can insert it into the database. Query for it.
If the query is successful we can just call validateCursor again on our weather
values with our weather cursor. So much less code.
And that's it. Our test is vastly simplified and
it will be useful to us later on. So now we can try running this test again to
see if it still works after the refactoring. And there we are, test passed. Now,
it'd probably be useful for us to actually print out some more data so
we can see what was being tested. But, this simplification to our
test is going to help us as we move forward. We're not done with testing yet.


34 - Simplify Tests
===================

Here's how I ended up simplifying things. I'd love to see great testing ideas.
So, if you're particular to what you've created, post your solution to
the forums. I'm starting off refactoring things a little bit. What I'm
going to do is I'm going to add a function to get the location content values.
This will be useful when we actually try to use these tests for
some other things later on. And there we are. A function that'll
return location content values. I'm also going to pull the city name out so
we can use that for some validation steps later on. And there we are. It's now
inside of the function. And then we're simply going to call that function to
get our location content values and we can delete all those lines of code.
Now, all of these columns, as I said before, are not very important, so
we can delete them. And we can also null out the value in our query.
The next thing I want to do is actually fix this validation step. What we can do
is we can create a function that relies on the fact we can get the map and
set from our content values. And then we can simply iterate through those.
Then we can see if the values that we used to create the record actually past
the values in the cursor that are returned. Now, back to our task. So,
there's a lot of unnecessary code here now. All we have to do is call
validateCursor with our values and our cursor. All right. So now we've
converted the first test. We can do the same thing with the second test as well.
First of all, let's do the same abstraction we did before. Now, of course,
one of these values isn't going to be static, so we have to add that in.
So now we can say, content values equals getWeatherContentValues.
With our all important location row ID. Remember, our tables are linked.
And then we can insert it into the database. Query for it.
If the query is successful we can just call validateCursor again on our weather
values with our weather cursor. So much less code.
And that's it. Our test is vastly simplified and
it will be useful to us later on. So now we can try running this test again to
see if it still works after the refactoring. And there we are, test passed. Now,
it'd probably be useful for us to actually print out some more data so
we can see what was being tested. But, this simplification to our
test is going to help us as we move forward. We're not done with testing yet.
