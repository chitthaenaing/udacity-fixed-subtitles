
01 - Introduction
=================

In lesson one, we built a new project with a basic UI backed with mock
data. In this lesson, we'll hook sunshine up
with the Cloud, replacing the mock data with
real weather data using the OpenWeatherMap API.
To do that, we'll learn about the Android
permission system, Network I/O, and how to move
time consuming tasks off the main UI thread.


02 - Review Data Provided by Open Weather Map
=============================================

On that note, we're going to make our sunshine app
to the OpenWeatherMap service. OpenWeatherMap provides a free weather data
and forecast API. And the site is linked below, so
you can follow along. By scrolling through the site,
you can see what type of data is available. It
also has example queries so you can click on this
one to see what the weather is like in London.
It includes a user friendly description of the weather
condition, such as broken clouds, in this case. And
we'll use this later in our app. For the
full list of weather conditions, you can go back to
the main page. And scroll down. And then, click
on this link here for the Weather Condition Codes.
And on this page, we have the full list
of weather conditions. It contains everything, from scattered clouds to
freezing rain, or even volcanic ash. I don't know
about you, but hopefully that doesn't happen too often.


03 - Try Out Web Queries
========================

Take a moment now to become familiar with the API by going
back to the main page, and looking at the different queries and
the different parameters supported. Just modify the query params in the URL
address bar. Check the box floating in the clouds when you're done.


04 - Find the Query We Want
===========================

Now, we should be able to figure out the most
appropriate URL that our app should use. Based on the
UX wireframes that we saw earlier, you noticed that we'll
want one week's worth of data. For this quiz and for
the example code that I'll be showing you later, we're
going to be using the postal code for Mountain View which is
either specifying the postal code, city name or the latitude, longitude
coordinates. Next we want their format from the server to be in JSON format,
and we want the units to be reported in metric. But our UI will
be able to display Celsius or Fahrenheit
based on user preferences, and we'll just
do the conversion ourself. Enter the URL
that fulfills these requirements in the box below,


05 - Find the Query We Want
===========================

For the Mountainview post code 94043, this is what the query would
look like. We set the mode to be JSON format, temperature units
to be in metric, and the count param to be seven days
of forecast data. Let's take this url and use it in our app


06 - HTTP Request for Weather Data
==================================

To fetch the data from open weather map within an
app, first we need to make an HTTP request to the
API at the URL that we decided on earlier. Then we
need to read the response from the input stream to get
a JSON string. It hasn't been parsed yet, but we'll worry
about that later. Then we clean up by disconnecting the connection
and closing any input streams. We also log any errors that
we catch. Since it's a lot of networking boiler plate code,
we've provided you with a code snippet
that handles these steps. See the link for
the GitHub gist below. After you've taken a
look at it, check this box to continue.


07 - HTTP Requests
==================

In the sample code provided you'll notice
that we create a Http Url connection. To
use Http to send and receive data over
the network, we have two clients on Android.
The Http Url connection class, as well as
the patchy Http Client class. Both options support
Https, streaming uploads and downloads, configurable time outs,
IPB 6 and connection pooling. We recommend the
Http Url Connection class because it's general purpose and light
weight, and it's been optimized to suit the needs of
most Android apps. To learn more about connecting to the
network, see the training guide and blog post link below.


08 - Logcat
===========

You'll also notice in the sample code that we
have lines that call log.e, which log error messages. With
the adb command line tool and with the device plugged
in, you can type adb logcat to see the debug
and error messages that are being printed to the logcat.
For more details on the logcat options, see the link
below. Another way to view the logs, is to use
Android Studio. If you click on the Screen Android button,
it launches the Android Device Monitor. Which includes the
DDMS tool that Rado mentioned earlier. Then click on a
device and you can see that our app here, is
a running process. If you click on the LogCat tab,
then you all these log messages here, which is what
we saw with adb logcat. Now try viewing the logs
yourself with either of those methods. If you do both,
then you get brownie points. Check this box to proceed.


09 - Logging on Android
=======================

When you're logging a message in your app, you need
to determine what log level it should be displayed at.
Verbose logs should never be compiled into your app, except
during development. Debug logs are compiled in, but they're stripped
out at run time. Error, warn, and info logs are
all kept. And this is what the logging API looks
like. The first parameter is the log tag, which can
be any string that you want to identify the log message.
It's good practice to define the log tag as a
constant in your class. Usually it's the name of your class
or the app. The second parameter is the actual log
message. If we go back to monitor, we can see that
this is the log tag column and this is where
the log message is. If you click on this drop down
here you can filter the logs by log level. For example
you can click on Error to see all the error messages.
If you click on warn, you'll see all the warning
messages as well as anything more severe than that. The
same goes for the other levels. And verbose means that
you can see all the logs at all log levels.
As a PROTIP, you should avoid log spam for your sake and
the sake of other developers. The log buffer could fill up and important
error messages that you really need to see will either roll of the
logs or it will get lost in the sea of unimportant log messages.


10 - Network Call
=================

Cool. Now that we know that we can use
logging to debug our app, we're going to return to
this networking code snippet here and drop it into
the sunshine app to query for actual weather data. Open
up your project in main activity.java. In this class,
scroll down to the placeholder fragment class within the placeholder
fragment class in the on create view method, scroll down
and go ahead and add the networking code snippet here.
To get her blank, is pasted below again if you need it. You may need to enable
these options in auto import or add the imports
manually. See the instructions below. When you run the
app, you'll see that is crashes. Use log
cat to figure out which error is the guilty
culprit that caused the crash. We're assuming that for
this quiz question. You're using a device that's running
honeycomb or later. Use the emulator if needed.


11 - What caused the crash Solution
===================================

If you run it on your phone, your Apple crashed
because of a network got main thread exception. We found
this answer by connecting our device to our computer and
then checking Android device monitor for the error. You can
see here that this is our package name and this
is our process ID. If you search the logs for
that process ID then our error appears. It's triggered ultimately
by a network on main thread exception. If you're curious
as to what line of our code actually caused
this, you can scroll down the stack trace to
read more. This is our framework code that you
can skip and here's where it actually hits our
app. It's within the placeholder fragment class, in the onCreateView method. And
it happens in main activity.java file, in line 116. So
if we go back to the code, on that line you can see that urlConnection.connect
actually caused the error, and that we can't do that on the main thread.


12 - Main Thread vs Background Thread
=====================================

When we said that the framework didn't want us
to run network operations on the main thread, what
is the main thread? Well, Android apps run by
default on the main thread, also called the UI thread.
It handles all the user input as well as
the output, such as screen drawing. Thus we want
to avoid any time-consuming operations here, otherwise the URI
is going to stutter. Instead, kick off a background worker
thread if you have to do some long-running
work. This includes doing network calls, decoding bitmaps, or
reading, and writing from the database. Okay. So, somehow,
we have to move the networking code off the
main thread. But how are we going to do
that? Well there are several options, but let's look
for the name of the Android class that simplifies
background thread creation and UI thread synchronization, so that
the results from the background work will come back onto the
main thread, and then we can use it to update our
UI. Search online and figure out the answer to this question
and then enter the class name in the box. Here's some
advice for you. If you ever get stuck on how to
do something in Android and you can't find it on the
developer's site you can try to check stackoverflow.com. It's a question
and answer site that many Android developers use as a valuable resource.
So chances are likely that someone has
already asked a similar question that you have.


13 - Main Thread vs Background Thread
=====================================

If you answered Async Task, then you are correct.
To understand why we chose Async Task as the answer,
we can check the developer documentation. In this API
guide on processes and threads, we can scroll to the
Worker threads section. Right away, if you want to follow
along, the link is included below. Say, for example, that
you want to download an image from the network
at this URL and then you want to update the
UI so that it displays this bitmap. Well, creating
your own thread to download the image is a
valid option, but there's a lot of overhead to
handle in making you app actually thread safe. After
you download your image, you cannot directly update the
UI from a background thread. However, Android has several
options to run code that manipulates the UI to
run from other threads. In this example, yet another
runnable is created to get the bitmap result back onto
the main thread to update the image view. This solution
is a little cumbersome because you have to manage two
runnables here. To abstract away this complexity we can use
an async task and then to kick off the async
task. For example, when someone clicks a button, then you
just initialize the task, and then you can call execute
on it, and then pass in any parameters that are needed.
Notice that when you're extending the Async class, there's a
couple of generics that you need to specify. The first is
the type that will be passed into the do in
background method. So, if you want to pass in this image URL
that is specified string here and then in doing background
you'll get a string parameter. The second one is for the
type of object that you'll get when you get progress
updates as a task gets executed. We're not using that here,
so it's okay to specify that as void. And the third type is type of results
that we'll be sending back to the main thread through the onPostExecute method


14 - Which Thread for AsyncTask
===============================

The main advantage of the AsyncTask is that you get
to focus on your app logic, what you need to do
on the background thread, and what you need to do when
it comes back to the main thread. You don't have to
worry about the details of threads and handlers. Of the methods
that I showed you, only doInBackground is required to be implemented.
onPostExecute is optional as well as some other methods. For each
method, tell us whether it's on the main or background thread.
You have a 50% chance of getting each one correct or a 100%
if you actually go check the java doc, so go do that now.


15 - Which Thread for AsyncTask
===============================

In the documentation for Async Task, we can
scroll down to the section that talks about
protected methods. Here we see that different methods
are called on the UI thread versus on the
background thread. It also contains information on what
order these methods are called in. Relative to the
doInBackground method. For example, onPreExecute gets called on
the UI thread before doInBackground. So with that information
now, we can populate the answers to our quiz. As
we mentioned before, onPreExecute happens on the main thread. And
here you can do any setup work. Then doInBackground happens
on the background thread. While this is running, you can actually
call publishProgress as many times as you want, so that
you can pass information to the UI. So that it
can update an then tell the user that a certain
percentage of the work is done. Each time this is called,.
It triggers onProgressUpdate with some information. Then,
you can show a loading indicator in
your UI that says something's 10% done, 50% done, 100% done. And all this
happens on the main thread. And then, once all of this is complete in
the background thread, then it calls onPostExecute
with the results on the main thread.


16 - Move to AsyncTask
======================

Let's apply what we just learned by opening up
the MainActivity.java file within our project. We're going to take
this networking code snippet and move it over to
it's own AsyncTask, so it runs in a background
thread. The task is going to be defined within
this fragment class. But speaking of which, it's actually
still called PlaceholderFragment. Let's do a little bit of
refactoring now by giving it a real name. Let's call
it ForecastFragment. And then you can update it
in other appropriate places as well. We can also
move ForecastFragment into its own file that way
the MainActivity won't get so long and cumbersome. Within
ForecastFragment we should define a new inner class
called FetchWeatherTask which extends from AsyncTask. And then you
can move the networking code snippet here. After you
make the changes and run your app, it should
look something like this. There really should be no difference except it
doesn't crash now. In the next step we will add code to execute
the task. And later in the lesson, we'll do JSON parsing and updating
the UI. In the meantime though, make these initial changes to your code.


17 - Move to AsyncTask
======================

For the solution within forecast fragment, we
implemented FetchWeather/task, which extends AsyncTask. The generics
we use are just Void, and this is fine for now. Then within the doInBackground
method, we copied our networking codes snippet,
here. It's the same, except [INAUDIBLE] in
certain cases we return null instead of
setting the forecast JSON string to be null.
Previously, the code was an on create view, which expected us
inflate and return a view. So it's important that it got
to the rest of the code. Even if there was an
error in the networking code. Now that the networking code is in
a sync task, there's really nothing that comes after this in
the given background method. So it's fine to just bail early
whenever there's an error. Also know that for our log messages,
we've defined a log tag constant. At the top of this task.
This log tag is defined to be the name of the FetchWeatherTask.class. The
reason we use this syntax instead
of declaring a string FetchWeatherTask, it because
we want these two to be in sync. If you ever rename the
class then it will throw an exception unless you also update it here.


18 - Add Refresh Button
=======================

As we mentioned before, you're going to still see fake
data in the app. We'll need to add code to
actually kick off a background task from the main
thread. For debugging purposes, it will be nice if we
can execute the task any time we wanted by
interacting with the UI somehow. So, we're going to add
a Refresh menu option for debugging. A warning, though,
this menu option should not shift in the final app.


19 - Why AsyncTask is Not Optimal
=================================

Before we go any further, I need to
jump in here. Catherine has already said that
the refresh button is for debugging only, but
let's look at why. In real life, you
should always seek to eliminate the refresh button
from your app. A good app should work
like a good butler, giving you what you want before you even have to ask for
it.
Much like the Save button, it's a relic of a
bygone age. That's the best a reassuring safety blanket for those
of us who grew up with floppy disks. With the ability
to run background tasks or send messages directly from the server
to our app, there's really no reason to force users
to manually hit refresh. But the app is up to date
and sync with the cloud should be a given. But like
we said, this is for debugging, so like print line it's
acceptable for this particular purpose. Let's also take a look at
our background threading model, which of the following is potential an issue?


20 - Why AsyncTask is Not Optimal
=================================

Well, the transfer isn't happening on the UI thread, but
that's not a bad thing. In fact, it's good that it's
not happening on the UI thread. So that's not a problem
either. Data transfers are an integral part of most modern smartphone
apps, so that's not a problem. The real problem is that
the transfer is happening on a thread whose lifetime is tied
explicitly to a UI component, in this case an activity. So
if the activity is terminated by something like a screen rotation,
the transfer will also be terminated.


21 - Better Ways to Sync
========================

Because it's being created within an activity, it can be
terminated as simply as rotating the phone into a different
orientation. So, should only ever be used for tasks whose
lifecycle is tied to the host activity, and is expected
to run for only a second or two. On mobile,
it's unwise to assume that even the most trivial network
access is going to happen quickly. So a better approach
would be to use a service. An application component without
the UI that's less likely to be
interrupted. Possibly scheduled using an inexact repeating alarm.
Even better, Android has a specialized solution know
as Async Adapter. And it's designed especially to schedule
your background data syncs as efficiently as possible. Better
still would be using Google Cloud Messaging. This lets
you notify your Async Adapter of changes on the
server side. So you're only ever initiating network requests
from your app when you know there's something to
be updated on the server. For now we're concentrating
on making our app work when it's in the foreground. But we'll return to
look at these approaches to invisibly updating
your app from the background a little
later. For now, keep in mind that the Refresh button and the new thread
solution is just a place holder while we hook up the rest of the app.


22 - Menu Buttons
=================

Thanks Raido, with that in mind let's add
a Refresh Menu button to our app. For temporary
purposes only though, otherwise Raido would be mad at
us. On Android, menu options are defined in XML
and they can be declared for fragments or
activities. When the fragment or activity is created, it
inflates this XML into the actual menu items in
the app. You'll see that there are Action buttons
which are menu items that appear in the Action bar,
such as this Search Menu item. This space is reserved
for the most prominent actions in your app. Then anything
else that's less important falls into the overflow menu by
tapping on this button with the three dots. These menu
items are ordered from most frequently used to least frequently
used. And on larger devices that have more screen real
estate, you can specify that some of these menu items can
actually go into the Action bar if there's room. If
you go back to our project in the Resources folder
which is called res, there is a Menu folder and
inside that there's a main.xml file. If you open that up
you see the menu layout XML, and that there's a
single menu option defined for Settings. It will never show up
as an action in the Action bar, meaning that it
will be in the Overflow menu. You can verify this by
checking the app on your phone. To define the ordering
of menu items, you can just add multiple items to this
XML file, and then they will show up in that order
in the app. If you don't like the order though, and
you want to explicitly set it, then you can specify
this order in Category, value. Right now it's set as 100,
so that the Settings menu will be at the bottom of
all the other menu options that we define in our app.
The only Menu option that should show up below the Settings
menu is the Help menu. Check out the link for more details.


23 - Refresh Button
===================

Since the fetch weather task is in the fragment, the
menu option to trigger that task should be inside the fragment
too. That means we should create a new menu layout
for the fragment and we'll call it forecastfragment.xml. The file will
live within the menu folder under the Resources directory. Inside
that file, we'll declare the new menu option for refresh and
give it this menu item ID. We'll also need to define
a string label for this word, Refresh. We're taking this one
step at a time, so just make the menu xml
file change in this exercise. If you compile and run
the app, there'll be no visible change. It won't show
up on the device yet because we haven't inflated the menu
item. Inflating it will be in the coding test after
this. See how the menu main.xml is implemented as an
example. For an additional hint, see the menu training guide
below. When you're done, you can click on this box here.


24 - Refresh Button
===================

Here's the code for the Refresh button in
the forecastfragment menu XML file. We want it to
appear in the overflow menu, so we set showAsAction
as never. We also declare the string. You can
find the string here in the strings.xml file, which
is located in the values folder, which is in
the res folder. You can get your strings.xml file
translated into different languages. This adds additional values folders,
under Res. For example, you would have
values-es, for Spanish, values-fr for French. And
within each of those values folders, you
would have a strings.xml file, which would
contain the localized strings for that language.
Since translation does cost money, as a
pro tip, you can mark strings that
you don't need translated using translatable=false. For example,
if it's a proper name or if it's for debugging like this one,
then you can indicate to the translator that they can skip over this strings


25 - Refresh Button Behavior
============================

Now that we have the menu XML, add the code in
the fragment to inflate it. You can leave a placeholder for
when the Refresh button is selected. This is what it should
look when you're done. You should see the Refresh menu item there,
but tapping on it doesn't do anything. That's fine for now.
If needed, you can look at mainactivity.java for reference. If you get
stuck and don't see the menu item appear, try to answer
this question first: what fragment method do you need to call in
order to report that it has menu options to display?


26 - Refresh Button Behavior
============================

The solution is that we need to call
Fragment.setHasOptionsMenu(true). That way, we'll
get appropriate options menu callback
methods in the fragment, so that we can
inflate the menu and for when a menu item
is selected. And this is where the fragment
inflates the menu. Remember from before that in the
ForecastFragment class, we have a public empty constructor, and
we also override onCreateView? We also define fetchWeatherTask here.
Now, we're going to overide an additional fragment life
cycle method called onCreate. This is where the fragment
is created, and this happens before the onCreate view
method, which is where the UI gets initialized. So, in
onCreate, we're going to call setHasOptionsMenu to be true to
indicate that we want call backs for these methods.
When onCreateOptionsMenu is called, we're going to inflate the menu
layout that we defined earlier called forecast fragment. We'll also
get notified when a menu item is selected. When a menu item with
ID action refresh is called, we're going to
return true for now. We're going to go
over the activity and fragment life cycle methods in more detail in later
lessons, but if you want, you can read the documentation linked below for now.


27 - Execute AsyncTask
======================

Now modify the code so that when the Refresh menu item
is selected, it executes the FetchWeatherTask. But hang on. When you
tap on the Refresh button, it actually crashes. Why don't you
go check the logs to see what error is causing this.


28 - Execute AsyncTask
======================

In the forecast fragment class, we modify the
onOptionsItemSelected method. When the Refresh menu item is selected,
we create a new FetchWeatherTask and then we call
execute on it. While the call is no longer
blocking the UI thread because it's an AsyncTask, the
app will still crash. If we check the logs,
we see that the app crashed. This time though,
with a security expression. It says Permission denied and
ask if you're missing the INTERNET permission or not. And indeed,
we are missing the INTERNET permission, so we need to request it.


29 - Permissions
================

Let's talk about permissions, when each app APK is installed,
it's given it's own unique Linux user ID. And each
app runs within its instance in the Android Virtual machine.
[SOUND] As a result each app is completely sand boxed.
[BLANK_AUDIO]
Its files processes and other resources are inaccessible
to other apps. This sandboxing approach is used to
ensure that by default no app can access sensitive
data or perform actions that could adversely impact other
apps, the OS, or users. Things like accessing the
internet, getting the user's location, or reading or modifying
the contacts database. Rather than asking the user each
time an app tries to access something sensitive you
declared permissions that your app requires in it's manifest.
You can get a list of all of the permission
your app may require here at the Android developer site.
So, why not just ask for all the permissions and
never worry about it again? Well, when users install
your app their greeted with this dialogue which lists all
of the permissions that you've requested along with the more
user-friendly descriptions. Nothing says this app was built by hackers
who want to steal my data, rob my house, clear out my
bank account, and take and share nude pictures of me like
a weather app that wants access to every permission on the
device. Best practice is to request the absolute minimum of permissions.
Every time you need to request a new permission, take it
as a flag to stop and consider if there are other
alternatives that might achieve the same goal. For example, using an
intent to launch the camera app, rather than accessing the camera, directly.
Later, we'll look at ways to share your data with
other apps, using intents and content providers to create bridges
between these apps sandboxs. You can declare and use those
permissions in these instances to
similarly restrict access to sensitive information.


30 - Permissions in the Manifest
================================

Take a moment to explore the
security section of developers.android.com. Which of these
following activities can only be done
after declaring a uses-permission in the manifest?


31 - Permissions in the Manifest
================================

While declaring a commission is required for using a
camera, making a phone call, or accessing contact details. You
can do each of these things using a system app
as an intermediator. Because the camera app, dialer, and contacts
app will each be used to provide the data
you request users have the chance to cancel the action
you initiated giving them run time ability to refuse your
access. However, UI can only access the user's current location
if it explicitly declares a user's permission.


32 - Adding Internet Permission
===============================

Thanks Fredo! So, like old times, we're going to
need to ask for permission. Go ahead and change
the Android Manifest file, to request the Internet permission.
And then tell us the exact string name of
the permission that you used. Run the app
again, to ensure that it doesn't crash. You can
also log the output of the network call, to
verify that you got the weather data back correctly.


33 - Adding Internet Permission
===============================

The answer is android.permission.INTERNET. I'll show you now,
where it's located in the code. Under the
main folder open up the Android manifest.xml file.
Within the Android manifest file, we declare uses
permission, with the name of the permission. We
verified that the data returned is correct by
adding a verbose log statement for printing out
the forecastJsonStr. This is in the fetch weather task.
We can verify that it's going to show up in the logs. For this example, I'm
going to use the command line. These are the real time logs, and then if I
hit refresh, then I see the weather data here. The verbose log, and this is our
log tag fetch weather task. Here's the forecastJsonStr,
and this is all the output from the server.


34 - Postal Code Param
======================

Great work. Thumbs up. The data has landed safely on
our phones. But I was thinking about it, and actually
instead of hard coding our post code in, we'd really
[SOUND] like our sun train app user, to be able
to change their location in the settings. So let's make
the FetchWeatherTask more flexible by having it take as input
a postal code parameter. While we're doing that, we should
also take this opportunity to do a little bit of refactoring.
Instead of concatenating the strings for the server query
URL, we should use a UriBuilder Class to build
up he URL. We can declare a base URL
and then append each pair of query PARAM and
PARAM values onto it. This includes PARAM's for post
code. JSON format, metric units, and date count. This'll
make it easier in the future if we ever
have to make these options configurable by the user.
If you want to verify that the URL is built up properly. After
you do these tasks, you can use verbose logging to print it out.


35 - Postal Code Param
======================

In the Forecast Fragment, we want to modify the
FetchWeatherTask, so that we can pass in the
postal code parameter when we execute that task.
You can click on this class, to jump
to where it's defined or you can use this shortcut. And here you see that we
modified the generic input type, to be a
string so that the doInBackground method receives string params.
Now, we only passed in one string param,
can read out the zeroth position in the params
array, to get that postal code. Notice that we're
using the UI builder here, and we're appending query
parameters, one by one. We define constants for these
query params, as seen here. And for the values,
we also define them up here. You can imagine
that in the future these might be configured outside
of this class, but for now we're just going
to define them here. To verify that this URI
was defined properly, we add a verbose log command
to print out the built URI. Let's verify that
the log we added actually shows up. In overflow
menu we tap Refresh. Then we see the log
tag FetchWeatherTask with our UI printed out. It says


36 - Identify Desired JSON Attributes
=====================================

The URL that we built looks good and the JSON
string from the server also looks good. However, it still is
one long string. Let's look at it more carefully to see
what data we should extract from it. In order to make
sense of the string that comes back from the server, we
can put it through a JSON formatter or a JSON prettifier.
If you Google for those terms, you'll be able to find
a helpful tool such as this one. Here, we pasted the
result of the web query. Then, it formatted the output for
us showing us the hierarchy of these elements. Now based on
the wireframes that you saw for the Sunshine app, can you
tell us which of these leaf nodes we care about extracting?
Even though we will have to traverse through parent nodes, for
the purposes of the quiz on the next screen, we're just
looking at the leaf nodes. Make your selections below on which
attributes we'll need in order to display the list of forecasts


37 - Identify Desired JSON Attributes
=====================================

And here the attributes we care about to display a list of forecasts in our app.


38 - Parse Out the Max Temp
===========================

For the next step, let's do an exercise that focuses
on extracting out the value of one of those attributes. We're
going to do this exercise a little bit differently. You'll implement this
on the Udacity platform. And we'll be able to verify that
your code is correct. After this video, you'll see an IDE
like this. Where it contains some starter code, as well as
some j unit tests in a separate tab. In particular, we
want you to implement this function, to get the max temperature
for a day. You're given a weather JSON
string, formatted the same way as you would get
from the Open Weather Map service. And you
also get the day index that we care about.
Feel free to play around with the code
in Android Studio, or a separate development environment, before
you paste your answer here. Or you can use
the Udacity IDE. Whichever feels more natural to you.


39 - Parse Out the Max Temp
===========================

For the solution we look for the list array.
Each element in the list represents one days forecast. Given
the day index we find the temperature node and then
the max temperature. And this is what the code would
look like. We change the JSON string into a JSON
object. Then we look for the list array. Within that
we find the day we care about. Then we look
for the temp node. And then find the max temperature.


40 - JSON Parsing
=================

Once you've learned how to pars JSON data, it's pretty
straight forward to be able to pars the rest of the
fields that we need. Since this isn't a course on
Java or JSON, we're going to provide you with the parsing
code in the gist below. And this is what the
gist looks like. There are three helper methods. The first is
for formatting dates. The second is for rounding temperatures. The third
is for turning a forecastJsonStr, into an array of weather forecasts.
Update the Fetch Weather Tasks to use these helpful functions. And
the do in background method should return a string array of
forecasts. You can log the output to check that the array
is correct. The format of one day's forecast should look like this.


41 - JSON Parsing
=================

We want the FetchWeatherTask to return an array
of string forecasts. That means we need to
modify the return type of the async task.
Then in the doInBackground method, after we've read
in the input string, we perform this code.
We call the helper method getWeatherDataFromJson and we
pass in the forecastJson string as well as
the number of forecast days. We also catch
any Json exceptions if there's a problem with parsing. We
wanted to verify that the string array of forecast data
is correct. So, within the getWeatherDataFromJson method, we added some
logging statements to print out each element of the array.


42 - Update the Adapter
=======================

From the logs, we know that we have the right forecast
data and it's in the right format that we want as
an array of strings. So it's finally time to update the
UI. Think back on how AsyncTasks are able to pass data
back onto the main thread. You can hit Ctrl+O to see
the list of available methods we can override in AsyncTask. If
you click on any of them, it will be prepopulated in
the code for you. Then, you can update ArrayAdapter with the new
data that was retrieved by the AsyncTask. As a hint, you
can make the ForecastAdapter be a global variable. That way, you
can access it from within the FetchWeatherTask. Make sure that this
is not a static class, otherwise, you won't be able to
access the member variable from the forecast fragment. Then, go ahead
and compile and build the app. When you run it, and
you hit the refresh button, you should see a week's worth
of weather data for your location. Once it's working, you can
remove the verbose log statements so you don't
clutter the logs. As you're working on this
code, if you see an unsupported operation exception,
make sure that when you're creating the fake
data, that when you initialize ArrayAdapter, you passed
in a list of strings, and not an
array. That way you can call the clear
method or the add method on this list collection.


43 - Update the Adapter
=======================

The solution is to override the onPostExecute method
of the AsyncTask. And this runs on the main
thread. We received the string array of forecast results,
which came as a return value from the do
in background method above. First, we clear the ForecastAdapter
of all the previous forecast entries. Then we go
ahead and add each new forecast entry one by
one to the ForecastAdapter. This is what ultimately triggers
the listView to update. Note that if you're targeting Honeycomb
devices and above, you can use an addAll method here,
so you don't have to add them one by one.
You can just add it once and update the listView once.
Once you hit Refresh, you'll see the weather forecast for
the next seven days for your location. Even though we hit
Refresh, we don't have any verbose logging statements being printed
out here. We don't need them anymore, because we have a
way to check that our code is correct by looking at the UI now.


44 - Source Code for ArrayAdapter
=================================

Do you remember this diagram from lesson one? The adapter
has a reference to the raw data. And has instructions on
how to build each list item layout. These layouts eventually make
it into the list view here. But what happens if any
of this data changes. Let's say for example, there's a fourth
contact named Don. The adapter knows how to create a list
item layout for Don. But now the list view is outdated
because it only has three items. Somehow, we need the adapter.
To notify the list view that things have
changed. And lucky for us, there's a method
called: adapter.notifyDatasetChanged( ). This notifies any attached observers,
like this one, that the underlying adapter data has
changed. Then the list view needs to populate
its children again. So it asks the adapter
how many items there are? And the adapter
responds with four. Then the ListView goes and fetches
each one individually until it fills up the screen.
And it's through that method that the ListView is able
to get all its list items. But now you
must be thinking. How did we actually update the ListView
successfully, without adding this line of code in our
app? Well, with a ray adapter, whenever you add or
remove elements from it, it internally calls notifyDataSetChanged. That means
we don't have to do anything. But how can be
confirm this? Well, we can check the actual implementation
of the ArrayAdapter class in the framework. And here's
the code for the ArrayAdapter class within the Android
framework. You can follow along with the link below.
First, we're going to search for the Add method. Remember,
we added forecast strings. To the adapter using this
method. And sure enough we see that, the notify
data set changed method is called internally. But you
might be thinking what about this Boolean here. Well
let's look it up. If you scroll to the top
you'll see that, notify on change is declared as
true initially. As you go through your other references you'll
see that it's only changed here. In this set
notify on change method. Since we don't call this public
method, with the value false, then we know that we're
going to be safely notified each time the adapter updates.
Now let's go back to the add method. I want to
show you one more thing. You'll notice in the add
all method, it only notifies its observers once. Whereas in
the add method every time you call this it will update
it's observers. That means that our code actually is refreshing
the list every time. So, if you're targeting honeycomb of above
you can use this method to be a little more
efficient. In general don't be afraid to try and do this
on your own. As a general pro tip, if you're ever unsure about something,
feel free to go and check the
Android source code. Don't treat the framework code
like a black box. Take advantage of the fact that Android is open source. The
more you learn about the platform the
better of an Android developer you can become.


45 - Take a Screenshot
======================

Now that our app is showing real data, we should take
a screenshot of our app, so we can remember what it was
like when we made it this far. I personally like doing this
throughout the process of building an app, because then I can look
back, and see how it evolved. I'm going to show you one
way that you can take a screenshot of your device. Open up
Android Device Monitor and then click on the device here. Then, click
on this camera icon, to do a screen capture. When it pops
up, you can hit Save, and then you're done.
On certain physical devices, there are actually hardware keys that
allow you to grab a screenshot. For example, on the
Nexus 5, you can long press on the volume down
and the power key, at the same time. That takes
a screenshot. It gets saved to the Photos app and
then you can share it out to anyone you want.
You can show them what an awesome app you're building.


46 - Done
=========

And now you're done with lesson two. Woohoo. You've made
a huge step in being able to transfer data from
a server and bring it all the way down to
your device. Think about all the possibilities now of valuable information
you could bring down to users to be available at
their fingertips. And that's pretty powerful. Speaking about making an impact,
have you thought about what you're going to do for your
final project yet? You can choose any topic that you want.
Share your ideas in the class discussion forum, and
you can share with your friends on social media too.
Over to beta now, while I take a quick nap
on this Done pillow. I'll see you in lesson three.


47 - Lesson 2 Recap
===================

So now you've taken a UI with mock data
and learned how to update with the real data. You
learned how to use permissions, move time consuming processes
of the UI thread, and were introduced to the basics
of network I/O. Join us again in lesson three,
where we'll create more activities and learn how to navigate
between them. But first, come with me as I take
you on another voyage in to the history of Android


48 - Storytime Android Consumer Platform
========================================

As of July of 2013, one and a half
million new Android devices were being activated every day.
So it's hard to believe that just a few
years ago, I started all meetings with potential developers explaining
what Android was and convincing them that it was
worth their time and effort developing for the platform.
It wasn't until 2010 that we even shared numbers
to indicate the growth of the platform, 100,000 daily activations
in May of that year. Which quickly grew to
in 2012. That represents over a billion Android devices around
the world, which is why I don't start my presentations
asking if anyone in the room has an Android device
anymore. To me though, the biggest surprise in those early
days was how much effort was required to convince people
that developing for mobile at all was a worthwhile investment.
A huge number of people use their phones to go
online and many of them use their phone as the main
way to do so. One study by the Pew Research
Internet Project in 2013 found that nearly two-thirds of mobile owners
use their phones to go online, double the rate found
in 2009. And that a third of phone users use mostly
their phone to get online. Think about that for a second.
These portable phones have become pocket computers constantly connected to the
cloud to make the sum of human knowledge available
to us at a touch and a glance. It's no
wonder then that for a growing number of people,
mobile isn't how to get online when you're away from
your desk. It's actually displacing PCs as their primary
computing platform. The change is so dramatic and the trend
so clear that companies like Google, who depend entirely on
the internet, have a mobile first policy, ensuring that the
mobile experience is the first consideration when building
new products. This trend is particularly evident amongst younger
users who grew up using mobiles and especially
in emerging markets like Africa and Asia, where the
infrastructure to support ubiquitous broadband for desktops isn't
as readily available. I saw this firsthand when I
had the good fortune to visit Kenya a
few years ago. Ubiquitous phones and passionate, talented developers
weren't really familiar, but what really struck me was the degree
to which mobile phones had become an integral part of people's
everyday lives. It wasn't just checking email and posting to Facebook,
it was using [UNKNOWN] to buy everything from groceries to bus
fare. Mobiles was such a part of life that I heard
rather than buying someone a drink to show that you're interested,
you'd send them a phone credit so that they can call
you if they felt the same way. In the transition of
smartphones from tech accessory to a part of life to the
main way you get online to how you pay for things,
even how you flirt, represents a huge opportunity for us as
developers. The question is, how are you going to change the world?
