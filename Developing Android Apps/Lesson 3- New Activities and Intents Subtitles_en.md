
01 - Introduction to Lesson 3
=============================

Sunshine is now using real data thanks to
the OpenWeatherMap API. And it's time for us to
build out more of the UI. In this lesson
you'll learn how to create activities, create the structure
of your app, and learn how to navigate
between screens. And that means learning more about activities,
the intent framework and even learning how to use
activities that come from other apps within your own.





02 - What to do next
====================

Hey, Dan. want to see our weather app, so far, that we've been building?
 Pretty cool. But I want something more.
 Well, we're not done yet. We just started
two lessons ago. Give us a break. But fair
enough, there's still a bunch more work left to
do. What do you think we should do next?
 Well for starters, let's make the list
item do something when you click on it.
 Well, since you seem to be the expert on clicking on
list items, why don't you do it? I'm going to go eat some jellybeans.
 I think Catherine was mocking me there.
Speaking of mocks, let's go do some UI.





03 - Sunshine App UX Mocks
==========================

Sunshine looks something like this, but it doesn't show
most of the fun information we have about the
weather. We want to show a detailed view when
the user clicks on a day within the list.
Okay, that was a bit anticlimactic. It doesn't really do anything
useful yet. But it will contain all sorts of great weathery details,
things that weather nerds love such as barometric pressure and that
navigators love such as wind speed and direction eventually, but baby steps.





04 - List Item Click Listener
=============================

From the mocks, we can see when a list item is clicked we should
perform an action to open up the details page for the forecast. How do
we implement the click listener behavior? Take
a look at the ListView documentation. What
method is used to wire up behavior on a list item when it is clicked?





05 - List Item Click Listener
=============================

The answer is setOnItemClickListener. We should register
an OnItemClickListener with the list view via the
setOnItemClickListener method. Then, when an item in the
list view is clicked, the callback is invoked.





06 - ItemClickListener and Toast
================================

When the list item is clicked, use Android's Toast mechanism to display
the weather forecast string. A Toast is a pop-up that displays a message for
a few seconds before fading out. It's particularly useful for debugging, where
it can be used as a way to indicate status visually without altering the app UI.
We're going to put our list view, set item click listener into forecast fragment
inside of the on create view method. We'll just start typing
listView.setOnItemClickListener and
let the auto complete fill in the rest. Inside of this function,
we're going to add click listener, which we can instantiate in place. So
here's where we can call the code in Android to display the Toast. So, now that
you know where to place the Toast code, take a look at the Toast documentation.
Add a Toast when the list item is clicked that contains the weather information.
Hint. You can get the forecast by getting an item from the forecast adapter,
at the position given by the on item click listener.





07 - ItemClickListener and Toast Solution
=========================================

Done already? Let me show you how it's done. As I said before, we
get the forecast by calling getItem from
the forecast adapter. And here's how we actually
display the toast. Note that the context
is actually available through getActivity in our
fragment. However, it won't do anything unless
we also show the toast. Displaying a toast
is useful to see how an Android interaction works. But this method should
really be used to start our detail activity. This is what we'll do next.





08 - Create New Activity
========================

You're going to use Android Studio to create the
detail activity from the blank activity with fragment template.
The template asks for the hierarchical parent of detail
activity, which defines the up button noted by the
app icon, and a left pointing carrot here. But
what does up mean? If you mapped out a
hierarchy of all screens in the sunshine app, the
root of the tree would be the main activity, and
it's child would be the detail activity. So, the
app goes to the main activity. While the back button
can achieve the same effect, it can also bring
you back home or to another app, depending on your
history of screens. Up will always navigate you within
the same app to the parent, regardless of how you
arrived at this screen. Create the detail activity now,
and look at how the activity is declared in the
Android manifest file.





09 - Create New Activity
========================

We start by right-clicking on our package name to create the new activity from
the Blank Activity with Fragment template. So
for name, we use DetailActivity, and again, the
hierarchical parent of this class is going to be our main activity. So as you
can see, we now have a new
DetailActivity along with the XML for activity_detail and
a fragment associated with that activity. Great! You're
done. But we still aren't launching our new activity.





10 - Intents Framework
======================

To move from one activity. In this case,
the MainActivity in our sunshine app, to another, here
the DetailActivity, we call StartActivity. But note that when
using the StartActivity method, rather than specifying the class
name of the activity to start directly, that
class name is packaged explicitly within an intent. The
basic premise is simple. Whenever you need your app
components to communicate with each other, or the system,
you use intents to identify the
target destination. Starting activities within your
app is the simplest example. [SOUND]
With your intent explicitly indicating the target
using the context and a class name of the activity. Intents that use the
name of the component you're targeting directly, are known as explicit intents.





11 - Intents as Envelopes
=========================

I like to think of intents as envelopes
each one includes who, or in this case, which component you want it
delivered to. And there's room for a small amount of data to be delivered,
packaged as extras.
Primitive tuples that will be available to the application
component that ultimately receives and opens this intent. So an
explicit intent, explicitly specifies the name of the recipient, as
you can see on this envelope. Implicit intents are a
lot more interesting. Here, we don't specify the name of
the class. We don't even know what it is. So,
how do we indicate which activity to launch? Well, you
know those stories you sometimes hear about letters with fake
addresses like these that somehow still find their way to the intended
recipient? Well the intent resolution system on Android, works the same way.
Finding an activity, capable of performing an action
you specify on the associated data. So, rather than
specifying the name of a class, you indicate
an action you'd like an activity to perform, and
on what data that action should be performed.
Like this, where we want to address an activity
that's capable of viewing a website. And because intents
can cross through application boundaries, the activity that started,
may not be part of your app. So,
you can include functionality like this, browsing websites or
making phone calls or choosing a contact from your address book. All
from within your app, without you having to implement any of it yourself.
You don't even need to know about the app
that will ultimately service your request. You can find
details of some of the intents supported by native
apps in Common Intents page of the Android developer site.





12 - Launch DetailActivity
==========================

Thanks, Rado. Now that you understand what
intents are in glorious metaphor, let's use them
to do something useful. We'll replace the toast,
with an intent to launch the detail activity.
You'll need to pass in weather forecast data for the detail activity to
display. For more information, see the intent
extra constants defined in the intent class.





13 - Launch DetailActivity
==========================

The solution is to lauch an explicit intent to
the Detail Activity Class. We'll go to On Create View,
where our On Item click listener is defined. To
pass in the Weather Forecast information, we can use an
intent extra, which is a key value pair in
the Intent. We use the key name, Intent.extratext, but we
can use any string for the key. As long as
we use the same key when reading the information out.
Just as in the toast, we obtain the forecast
data from the current list item, by calling get
item on the adapter at the given position. Once
we've added the intent, we just start the activity.





14 - Display Content in DetailActivity
======================================

Once the Detail Activity is launched, it shows Hello World. Instead,
we want to read the forecast data from the Intent, and display
it. Make the appropriate changes in the app, so it matches
the mocks. Then, verify the behavior by compiling and running the app.





15 - Display Content in DetailActivity
======================================

In the fragment detailed layout, a text view
is already defined. We have to assign it an
ID so that within the detail fragment of detail
activity we can use find view by ID to
find the text view. To determine the forecast
info that the text view should display. We can
access the intent that the detail activity was launched
by. We're going to use this inside of onCreateView.
[SOUND] Since we added an intent extra with the forecast
data there. We can read that intent extra back to display in the text
view, as you can see we pulled the forecast string from the
intent find our rootView and then set the text to that forecast string





16 - It works
=============

Woo Hoo. Clicking our list items works.
 Woo Hoo. [CLAP]
 Our app is awesome.
 Yeah.
 But clicking on the settings menu does nothing. Whomp whomp. #sadtrombone.
 Mm, don't do that again. Let me save
you from Dan. And let's go build the settings now.





17 - Settings UX
================

Let's go through the wire frames for the settings UI.
Within the detail activity in the overflow menu, there's a settings menu item.
When you tap on the setting's menu item, it opens up the settings activity.
If you tap of location, it pops up a dialog, where you can enter in your
postal code. Feel free to use your preferred method for inputting location,
such as city name. Just verify that the data coming back from the server is for
the location you expect. Note that there's no validation on this string input.
If you're trying to make this app a production app, you would want some kind of
air handling here. If you tap on pick your units, it opens up a dialogue
where you can choose to display temperature within the app as wither metric or
imperial units. This is basically just Celsius or Fahrenheit, and
that is what this settings activity does. Remember that you don't want to make
everything a setting. If you're discussing a feature with a teammate and you
can't decide on something, don't compromise by just making it a user setting.
There are instances where making a difficult product decision now will be better
for the user, because then they won't have the burden of having to decide what
the value of the setting should be. And remember, there's always an opportunity
in an app update to add the setting later. If you add it to begin with, it's
infinitely harder to take away the setting because some users might get angry
with you. If you're having trouble deciding if something should be a setting or
not, you can check out the setting section in the Android design guide,
which is linked below. It contains a workflow diagram that could help you.
Very few roads often lead to making something a setting.





18 - Preferences
================

Open up the Developer API Guide for Settings, if you want to follow along.
If you've pondered it carefully and you've decided you really want this to
be a setting, this is what you do next. Android provides a way to display
a hierarchy of preferences, including headers, such as the ones shown here.
The value of the preference is called the preference summary, and that's seen
here. It looks almost like the subtitle text. The preference also provides UI.
To allow users to modify that setting. You just need to supply the strings, but
you don't have to implement the UI like the spinner here.
Common preference types are the check box preference, list preference,
edit text preference. We can see some examples in the settings app under
developer options. Here's a bunch of preferences. This is an example of
a check box preference. This is an example of a list preference where you
have different radio button options here. Once the user changes the setting,
it gets saved in the default share preferences file. This stores a bunch of key
value pairs of primitive data. An example integer preference could be how many
days of e-mail history should be saved locally on the device. You could have
a string stored integer preferences for what the currently selected account and
device is. Then at any time you can fetch the value of this shared preferences,
as long as you pass in the name of the key that you want.
You can also use shared preferences outside the context of settings as well.
This information is located in the developer API guide, for
storage options, which is also linked below.





19 - Create SettingsActivity
============================

We'll take advantage of what the framework offers in order to build our
SettingsActivity. Since we're supporting Gingerbread devices,
we'll use a preference activity class. If you're targeting Honeycomber later,
you'll want to use the PreferenceFragment class.
Create a new SettingsActivity class from the gist provided below.
Update the AndroidManifest to declare this new activity. You can see the way
detail activity is declared as an example. If you see that APIs are deprecated,
that's okay. That's a side effect of using PreferenceActivity, in order
to target Gingerbread devices. When you create your app for your final project,
you can use the wizard in Android Studio to create a new SettingsActivity.
The code the wizard provides will make your app backward compatible,
by using a combination of preference activity and PreferenceFragment.
Our gist is meant to be a simplified version of that for learning purposes. So
later, you'd be able to understand better what the wizard is doing.
Check these items off when you're done.





20 - Create SettingsActivity
============================

First, we create our settings activity, and this is the code that we got
from the gist. Then, in the Android Manifest, we declare the settings activity.
We give it an activity named Settings, which is declared in the Strings file.
We also declare the main activity as the parent activity. That way,
when you hit the up button in the settings activity,
it will return back to the main activity.





21 - Launch SettingsActivity
============================

Now it's time to launch the SettingsAactivity from the menu item.
We need to launch it from two places, the MainActivity and the DetailActivity.
Afterwards, compile and run the app to make sure that it works. This is what it
should look like. From the MainActivity, if you tap on Settings menu,
it should bring you to the SettingsActivity. It is expected for it to be blank,
because we haven't populated it yet. Now let's try the DetailActivity.
If you tap on the Forecast, you can go into the DetailActivity and
then, you can go into the SettingsActivity. Complete the task and
then verify that your app behavior is like this.





22 - Launch SettingsActivity
============================

In order to launch the SettingsActivity, we modify
Mainactivity.java, in the On options item selected method.
When the Settings Menu Item is selected, we
create a new, explicit intent, to the SettingsActivity Class.
We call Start activity, to launch this intent.
We don't call StartActivity for result because we're
not expecting a result from the SettingsActivity. Similarly
in the detail activity class, when the settings menu
is selected, we launch a new intent to SettingsActivity.





23 - Location Setting XML
=========================

We have our settings screen but no settings.
In the next several sections you're going to be adding our two settings.
I'll walk you through how we add our first setting for
location, then you'll do the temperature setting on your own, so listen closely.
First we design a preference XML file.
The root element should be a preference screen.
In this example we have a check box preference and
a list preference inside of it.
In order to add the preferences XML file to our app,
we first need to create an XML folder under the resources res directory.
Right-click on the res folder, go to new Android resource directory and
then type in XML as the name of the directory.
For resource type you can use XML and then hit OK.
Inside this folder, we right-click to create a new XML resource file.
We call it pref_general,
and the root element is a preference screen as we saw on the developer's site.
And then our preferences file is created.
Then add the location preference.
Since the wireframes showed an editable text field to input user location as
a string, we make this an EditTextPreference.
If you want to learn more about all the possible XML attributes,
you can google for EditTextPreference.
First we specify a title for
the preference which will be the label that's displayed to the user.
Then we specify a key.
And this key is actually the key value in SharedPreferences.
Going back to the code, we also add these attributes to describe the Edit Text
field in the pop up dialog that was shown in the wire frames.
Going back to the code, we can also configure properties of the Edit Text field.
We specify the input type to be text, and we cap it to a single line of text.
It's bad practice to hard code these strings here,
so we declare them as constants in the strings.XML file.
These are the strings we declared.
This is the name of the label of the preference.
It's user-visible, so we add a char limit for translation purposes.
You also specify location which is the name of the key in Shared Preferences.
We don't want to translate it because we always want to find the key based
on this name.
We also specify a constant for the default location.
We also don't translate this because we need to send it to the server API
like this.
When you're done with these steps check them off and continue





24 - Modify SettingsActivity
============================

To make the preferences appear on the phone, we
need to modify settings activity. In the on-create method
we're going to replace this to do with some code.
We're going to call addPreferencesFromResource with the pref_general xml
that we defined. Then we need to bind the
preference summary to the value of the location preference.
This means that when the user changes the preference,
the summary value underneath the label will be updated.
If you look at the declaration of this method,
we see that for a given preference, it sets a
preference change listener on it. If you scroll to
the top, you see that the settings activity actually implements
the interface on preference change listener. That method is
found down below, where we override the on preference change
method. Remember that our location preference is actually an
edit text preference. So, it won't fall into this case,
but it will fall in this case down here. Then,
in the settings UI you can see that the summary
is now this value. Make these changes in your app.
See the instructor notes for the lines of code you need.





25 - Use SharedPreferences
==========================

Once the user's location preference has been stored. When do
we make use of it? We need the preferred location on
we query the weather service. Luckily, in lesson 2, we
modified fetch weather task to accept a location parameter. We hard
coded it to be the Mountain View zip code. But
now we should use the location stored in shared preferences. We
haven't explicitly shown you how to do this. So you might
need to do some digging around. On the Ander developer site
to figure out how to do it. Once
you've made the change, check the box to continue.





26 - Use SharedPreferences Solution
===================================

Within the forecast fragment class, when the refresh menu item is selected,
we read from SharedPreferences. Since they are key-value pairs, we get their
value stored for the location key. If there's no value stored then we fall back
to the default location. Then we pass the location into the fetch weather task.





27 - Update Data on Activity Start
==================================

On app launch, I know that you're probably tired of seeing the dummy data and
needing to hit the refresh button for real data every single time.
I'm going to show you how to display the data using the current settings each
time the activity is loaded. In forecast fragment,
we just refactored the code so that when the refresh button is selected,
we call this helper method, updateWeather. Now that it's in a helper method,
we can also call it from the onStart method. We over write onStart so
that the refresh happens whenever the fragment starts.
This will cause the weather data to appear. We'll learn all about on start, and
the rest of the android life cycle in the next lesson. At this point,
we can also remove all the fake data that we had in the onCreateView method.
Then we pass in an empty array list to the ArrayAdapter. Now we can test this.
We need to make sure that this is the first app launch and
that it wasn't already running,. So if it's already running, kill it.
And then tap to open the app,
and then you should see the actual weather data immediately. Under settings we
can change the location to be the pairs postal code. We hit OK, and
then return to the main list, and then we see that the weather data has updated.





28 - Temperature Units Setting
==============================

Now its your turn to build the temperature unit setting. This
will bring together a lot of the concepts that you've learned and
will take some time to implement, but we think you're ready for
it. Modify the app, so that the right temperature units are displayed
to the user. Use the assumption that we're syncing metric data from
the server. Eventually, we'll have a database on the device and we
don't want a mix of Fahrenheit and Celsius data in there. Hence,
we'll stay consistent by getting metric data from the server. And then,
we'll convert on the fly to the preferred units when
we update the UI. After you make these changes, I'll show
you what it looks like on the device. If you
go to settings, you'll see the temperature units preference. If you
tap on that, you see a dialog with two options.
If you switch to imperial and then you go back to
the forecast list, then you see that all the temperatures now
are in Fahrenheit. Once it works, answer this question. Which subclass
of preference did you use?





29 - Temperature Units Setting Solution
=======================================

The solution to this question is ListPreference. When the setting
is tapped, it presents a list of choices as a
dialogue. When a user selects an option, it's saved into
Shared Preferences. Here are the steps to do this. Within
the pref_general.xml file we add a list preference. We give
it a title, and this maps to the string temperature
units. We also need to give it a key for
the shared preferences. And so we specify that to be units.
If there is no value in shared preferences, then it
falls back to a default value of metric. The preference
also wants to know the list of all possible values
here. The two possible values are metric or imperial. So we
pass those in as an array, into entry values. Now
these are just constants for use within the logic of our
app, these are not the user visible strings. So we
need to create entries which is an array of user visible
strings that maps to each one of these possible values. To
have the temperature unit preference show up in the settings activity,
we modify the on create method. We add this line of
code for the temperature units setting. When the user makes a
selection in the dialog, we set the preference summary to be
the new value of either metric or imperial. Here, when we
receive a high and low temperature, we read from shared preferences
to know whether the temperature should be converted to imperial units or
left as metric. Then we return the formatted string for display in
the UI. That completes the code for saving the temperature unit setting.





30 - Debug Breakpoints
======================

I'll show you how we can add breakpoints in our code. Go ahead and
click on the screen bug icon, to attach the debugger.
We'll go to the Settings menu. In the SettingsActivity I'm going to add two
breakpoints, in the onPreferenceChange method. I'm going to add a breakpoint at
line 59 in the case of a ListPreference when it's about to set the summary. I'm
also going to add a breakpoint at line 63, when it's not a ListPreference and it
sets the summary. Let's go into the app to try it out. Tap on the Location and
change the postal code. If you hit OK, then it triggers the breakpoint to
stop at right here. These are the values of the variables at this time.
And we see the new postal code here. You can step over or into the method for
more details. Or you can hit Play to continue. Or Stop to stop the debugger.
Let's hit Play to continue. And then we see that the [UNKNOWN] has been
updated to the, the preference summary, is the new postal code. Let's try it for
Temperature Units. They tap here, and then select Imperial.
It triggers this breakpoint in the ListPreference case. We hit Play to continue,
and then we see that, Imperial is now the preference summary.





31 - Launching Implicit Intents
===============================

Hey Dan, we finished the settings. Do you want to check it out?
 Oh, great.
 Wait. What's with all this stuff right here?
 I'm glad you asked. All this stuff will help me demonstrate the
power of implicit intents. Leveraging other
apps to get functionality for your app
for fee. For example, why would you write a bar code scanning app,
when you can just launch an intent to do it for you. [SOUND].
 Or a camera app, where you can just launch
an intent to fire a photo. [SOUND] And of course, it would make no sense to
write a phone app just to dial a phone number, you would just call the intent.
 Wait, I need that.
 In other words, don't reinvent the wheel. Lazy programmers make fewer
mistakes. Now, let's go implement an
implicit intent. Raydo? Dan's being implicit again.





32 - Add Map Location Intent
============================

Add a menu option to view the user's preferred location on a map. It
will be handled by any maps app that's available on your device. If there
is no map app on your device, handle this case gracefully to avoid a
crash. You might want to recall that
we use SharedPreferences to store the preferred location.





33 - Add Map Location Intent Solution
=====================================

Done already. Let's go to the solution. In the menu layout file for the main
activity, called main.xml, we add another menu item
that will always be in the overflow menu.
We also define the string in the strings.xml file. The menu will be inflated in
the main activity class, so we can handle
the map menu item being inflated. By calling
helper method called open preferred location in
map. The helper method reads the users
preferred location from shared preferences. Then we
create a view intent, indicating it's location
in the data URI. The format of the data URI was from the documentation
page on common intents. Where you can append a postal code as a query parameter.
Finally, we start an activity with [SOUND] this intent. Note
that we only start the activity, if the activity resolves successfully.





34 - Intent Resolution
======================

So, how did the system know that the Maps
app could handle this Intent? The activity within the
Maps app included an intent filter like this one
within its manifest entry. It indicates that the Maps
app is capable of performing this view action on
all data which has a geo scheme. You can
use intent filters for your activities to define the
actions they're capable of performing, and optionally, the kind
of data that they can perform it on. So,
when you use an implicit intent to address or
request to start an activity, much like you did
with Catherine and the openweathermap.org link, Android resolves this request
by checking the intent filters of every activity that's
installed on the device until it finds the best possible
match. If multiple activities match, as in the case
with the web url, the user is given a choice.





35 - Share Intent is Awesome
============================

One of the most used implicit intents
in Android is the share intent. It's [LAUGH]
awesome. It's a great way to waste time
for both you and your friends. [SOUND] [LAUGH].
 Dan!
 You can leverage your favorite apps to share photos, texts,
videos and general links. And the best part is you don't have
to even know what the user's favorite apps are. Just tell
Android what data you want to share and it shows your users
a list of apps that will share it. Apps that
may not have existed when you app was written. To
make it even easier to share Android 4.0 added the
share action provider and we have it for previous releases of
Android as part of a support library. It operates using
the action send intent. If we use Raytoe's glorious envelope metaphor,
it would be addressed to anyone who could perform action
send. The data descend would be included with it as extras
either as text, or URI to the binary data. And
the MIME type would indicate what the URI points to.
Here's what the share action provider will bring to
our app: the glorious wire frame. There will be
an additional icon in the action bar which will
drop down a menu when clicked to reveal potential
share actions. Another icon will be added to the
right of this one that contains the last application
shared to. Add a ShareActionProvider to the fragment within
DetailActivity. It will share weather in the following format.
Yes, it's just the string we passed into the view plus #SunshineApp.





36 - Share Intent Solution
==========================

Here's our coding solution for the share action
provider. Lets start by adding a string. The
string will be used for the label for our action. Now we're going to add a new
menu resource. This resource will be for the
detail fragment. We'll start by adding the name
space used by the Android support library for
it's widget XML. Finally, we add the actual item.
This menu item contains the string that we
just added from the strings file. It's always
shown as an action. So it appears on
the action bar. And it's actionProviderClass, is the ShareActionProvider
from the Android support library. The Android support
library allows the ShareActionProvider to be used on earlier
versions of Android then 4.0. When it was
officially added to the framework. Next, let's go to
the DetailAactivity. We're going to be looking at the
DetailFragment within the DetailActivity. So the first thing
we can do in our fragment is to add a few useful things. Let's add a log
tag, a string for the share hashtag, hashtag
sunshineApp, and we're going to take the forecast
string and make it a member variable. Next
in onCreate view, let's actually populate our member variable.
And then finally, use it to set the text.
Lets create a share intent. This intent uses ACTION_SEND.
This flag activity cleared when task reset is somewhat
important. It prevents the activity we're sharing to from
being place onto the activity stack. What will happen,
if we don't have this flag, is when you
click on the icon to return to the application
later, you may actually end up in another application.
The one that's actually handling the share intent. When
FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET is used, it'll actually return you to your
application instead. We've set the type, test/plain, to let
Android know we're going to be sharing plain text. And
then we share our forecast string plus our hashtag.
Only one more thing left to do. We have
to add the menu to the fragment. To do
this, we have to set a flag that this fragment
has an options menu at all. Otherwise, it
won't actually call the onCreate options menu member
function. Finally, here's our on creat options member
function. As you can see, we're inflating the
detailfragment menu that we created earlier. Finding the
share item, getting the ShareActionProvider, and attaching our
intent to this ShareActionProvider. You'll want to update
this whenever the data you want to share changes.
And that's it. You've now added a ShareActionProvider into Sunshine.





37 - Broadcast Intents
======================

We've talked about using intents within Sunshine to start activities and send
data between apps. But what if you want to broadcast a message to
many apps? For example, the system will broadcast a message, saying that the
device is charging or that it's just finished rebooting. To do that ourselves,
we use the SendBroadcast method to broadcast an intent. And those broadcasts
are received by broadcast receivers, typically
referred to as simply, receivers. Using intent
filters, like the ones we saw earlier, they indicate which broadcast they're
interested in. So if we return to our envelope metaphor from earlier, in
the case of a broadcast intent, your
envelope is basically addressed to any broadcast receiver
that's interested in knowing that this event has
happened. And it does that using intent filter.
So intents, which are broadcast, are transmitted to every
app with a broadcast receiver that has an intent
filter telling the system your ideas are intriguing to
me and I wish to subscribe to end user.





38 - Intent Filters
===================

As you can see, the receiver itself is pretty
simple. Extend the broadcast receiver class and override the
onreceive handle as you can see here. Receivers are
lightweight components meant to listen for broadcasts, reacts, and complete
within five seconds that usually means simply starting a
service or sending a notification. To have your receiver
start listening for broadcasts, you need to register it
either through an entry in your manifest like this or
dynamically within another application component usually an activity,
like this. Then you define the intent filter that
specifies the broadcast event you want to receive.
You can do that either here in the manifest
or if you're registering your receiver with an
application component such as an activity, you can create
your new intent filter like this. The biggest difference
between manifest and dynamic receivers is when they're triggered.
A programmatically-registered receiver will only receive broadcasts while
the app is running. While a manifest declared
receiver will start your app specifically to receive
your broadcasts. For example, I usually code with
my noise cancelling headphones on, so a music
app like this might want to register a runtime
receiver to listen to the system intent broadcast
that announces when headphones are unplugged. That will allow
it to automatically pause playback, when your audio routing switches
from headphone to speakers. In this case, we can use
a runtime receiver, because the app is guaranteed to be
running if music is playing. On the other hand, if I'm
using Google Cloud Messaging to listen for new data from
my server, I want to register a manifest receiver that always listens
for incoming tickles from my server and will wake up
my app if it's being terminated to trigger the update code,
usually a sync adapter, which is something we will discuss
later in lesson six. There's a bunch of system broadcasts
you can monitor to help your app react to changes
in the system. You can see the system broadcast actions within
the intent documentation at developer.android.com. One great use for broadcast
receivers is to monitor for changes to internet connectivity or
charging status. Imagine your app has to upload a movie
or calculate the next 10,000 moves in a game of chess,
how might you use a receiver to wait until the device is plugged in
before starting that process? Should it be
a manifest receiver or a broadcast receiver?





39 - Intent Filters Solution
============================

You could register a listener for the action power connected
event and you should do that within your manifest. That
way, it can be called to start your app once
the device is getting charged, even if it's been terminated since
the last time you were using it. You can use
a similar approach to delay trying to send data until the
device has internet connectivity. Would it change behavior of the
user experience when you're connected to a car or desk dock?





40 - Lesson 3 Recap
===================

In this lesson we added new activities to our
apps and explored ways to structure and navigate between them.
And that meant learning all about intents and how
they're used in Android. Next up in Lesson 4, we're
going to learn about the activity life cycle and make
our apps that much more useful by saving data. We'll
begin to databases and learn about content providers, but
first let's take another journey into the history of Android.





41 - Storytime Android Distribution Platform
============================================

Android Market as Google Play was named when
it first launched alongside the first Android device
in 2008. A few days later, Android Market
opened for all app developers to register and
start publishing their apps without any review or
validation requirements from Google, OEMs, or carriers. For
us as developers, the ability to distribute apps and
the possibility of monetizing them through up front payments
in app billing and advertising directly to the owners of
over a billion android devices is one of the
most exciting opportunities made possible by the android ecosystem. Growing
up the dark days before smartphones and the internet,
if I wanted a new game or app I had
to go to a physical store and pay for a
cardboard box filled with floppy disks or CDROMs. As developers
a big part of our business plan back then
was how to get those boxes of our software onto
those shelves. Or in the world of mobile, how
to secure a carrier or OEM deal to get our
apps preloaded onto the devices. Today I can install
over a million apps of games created by tens if
not hundreds and thousands of developers via Google Play, all
with a click of a button. That's translated into tens
of billions of app installs. An incredible opportunity to
distribute apps to Android users in hundreds of countries.
Now, in the early days of Android, being fast
and launching first was a pretty solid strategy for success,
but as those first 50 apps quickly grew to
hundreds, thousands, and now over a million apps, it takes
a lot more than speed to stand out in
the crowd. These days the tables stakes are a base
line of quality and willingness to create great experiences
for all your users. Of course app quality can
be somewhat subjective, but over the years we've identified
the core criteria that users identify with apps of
high quality. And to make your life easier, they're
all available on the Android developer site for you
to review. This is the same checklist that the
merchandising team for Google Play used to ensure only apps
of quality are featured in their promotions and
editor's choice categories. And they include everything, from following
the app design guidelines to supporting different screen orientations,
tablets, and other devices. And of course ensuring that
your app is stable and performant. So, with over
a million apps already available in Google Play it's
no longer just a numbers game. We don't need
just more apps or games, or even more developers.
What we want is high quality apps from great developers, like you.
