
01 - Welcome to Developing Android Apps
=======================================

 Sure.





02 - Introducing Your Instructors
=================================

I'm Katherine Kwan, and I'm a developer advocate at Google. I started off by
working as an engineer on the Android apps team. I worked there for
three years on products like Google Play and Google Keep, the note taking app.
I'm continually blown away by what you can build on this platform, and
I'm particularly inspired by those apps that can truly improve people's lives.
When I meet people, they often tell me about their great app ideas. But
then they feel like they don't have the technical skills to actually build it.
And that's why I shifted over to developer relations so
I can help people like you really build something great. Throughout this course,
I'll be helping you implement your very first app.
[BLANK_AUDIO]
 My name is Dan Gauthan. I've been a developer advocate at Google for
four years and I've worn a lot of hats here. My goal is to keep us on track to
make Sunshine, something that had some resemblance to a production quality app.
I'll introduce Android design patterns and principles. Many of which are often
missed by new Android developers. You'll learn to lean on the framework,
focusing on what makes your app unique instead of reinventing the wheel.
[BLANK_AUDIO]
 I'm Rado Mya, and I've been a developer advocate at Google for five years.
I started working on Androids so long ago that back then there were no devices.
All we had to work with were an emulator, a beater SDK, and
an almost fanatical devotion to the platform. I'll be joining you from here,
my underground Android developer bunker, and my goal isn't to get you
up an running as quickly as possible. It's to teach you the context behind
the choices we'll make. And the best practices you need, to really understand
how to create great experiences specifically tailored to mobile devices. By
the time you finished this course you'll know more than just how to create yet
another app. You'll know how to build great apps that focus on the user.





03 - Are you ready for this course
==================================

[BLANK_AUDIO]





04 - Introducing Project Sunshine
=================================

In the course, we're going to build a weather app called Sunshine. The main
screen is a list of this week's forecast, with an emphasized presentation for
today's weather. With just a quick glance, I can decide if,
today is a stay inside, in code day. Or if I should code from the park,
instead. We have icons to indicate conditions, like snow,
rain, fog or sunshine. We can scroll through the list,
to see a long range forecast. With a click on one of the forecasts,
will bring up a detailed view containing the rest of the weathery goodness,
which also gives us the ability to share the weather with our friends.
We can go to Settings and change the unit of measure, as well as the location.
The app can even post system wide notifications, to let us know about weather
conditions without even entering the app. So, you might ask, why are we
building a weather app? We chose the weather app because it's a simple concept,
which touches most of the core APIs we want to teach and everyone can relate to
weather, unless you're in a bunker. By the end of the course, you'll be able
to use those APIs and concepts to build your own app for your final project.





05 - Course Goals and Prerequisites
===================================

We're expecting you to already be familiar with object-oriented programming, and
to have experience with Java or a similar language.
Personally, I got started with Android after years of Delphi and C# development.
As engineers, we're trained to map our experiences in one language or
SDK into new ones. Looking for patterns and shortcuts we can get up to
speed as quickly as possible. But mobile, and Android in particular,
have some fundamental differences that mean some of our experience and
intuition will actually make us worse mobile engineers. We like to think of
Smartphones as computers that fit into our pockets. In a way that's true, but
the computers from the 90s, like this Powerhouse I was using nearly 20
years ago. With limited internet connectivity, low speed CPUs, and limited RAM,
on top of which, they're all running on batteries. As we go through the course,
I'll be returning to each of these key mobile challenges. Looking for
places where you're experienced on desktop or
server, or even other mobile platforms might lead you in the wrong direction.
I will also be back to tell you some stories from the early days of Android.
Stories that describe how the Android platform has evolved. And nowhere is
that more readily apparent than here, at the Android Lawn sculpture garden.
Join me here, at the very heart of Android, at the end of this lesson,
to find out how this sculpture garden of tasty treats, also tell us the story of
Android's history. For now, let's get back to building that weather app.
So, we'll cross to Dan, to take a look at some mocks.





06 - Introducing More Sunshine
==============================

Thanks Raydo. But let's start thinking about Sunshine, meaning our app. I'm not
convinced I'm ever going to tire of the obvious puns our app name allows.
Remember, as we progress through this course and build this app together, you
should be thinking about the app you want to create. There are too many weather
apps already after all. By the end of the course, you could be ready to tackle
the final project. Using all your new Android know how to build your own app.





07 - Installing Android Studio
==============================

Thanks, Dan. As mentioned earlier, we'll be using Android Studio throughout this
course. It's an IntelliJ -based IDE that is optimized specifically for
Android development. It's still in beta development, so
your version may look slightly different, but that's okay.
The core functionality should still be the same. If you haven't installed it
yet, take a moment to do that now using the detailed instructions provided in
the link below. You'll also need to install the support library and that's also
included in the instructions. Note that the SDK is included automatically, so
you don't need to install that separately. If you're having difficulties, no
worries at all. We totally understand because it can be tricky to get everything
set up properly. You can search online for people who have a similar issue or
there's a troubleshooting link included below. If you still need help, you can
ask a question in our discussion forum as well. Once you install it, open it and
you can check for updates before proceeding. Check this box when you're done.





08 - Create a New Android Studio Project
========================================

Once you install Android Studio, you can open it and
check for updates before proceeding. Then we can configure Android Studio.
Open up the Android SDK Manager to see what tools, platform versions, and
components we have installed. At the time of this recording,
the Android L platform is available but it's still in developer preview mode.
That means it's subject to change until the official release. You can't upload
your app to Google Play if it's targeting this L platform version.
Hence, we want to use the latest stable platform version which is KITKAT or
API level 19. API level 20 is for Android ware or watches and not for
phones or tablets. Deselect all the other packages. And just choose the ones for
API level 19. We want the SDK as well as the system images for the emulator.
We also want the Android Support Library for backwards compatibility. For
the most up-to-date version of all the packages you need to install,
check the link below. Otherwise, continue with installing the packages.
Accept the license and then install. Then we can click and
create a new project. In the Project Wizard, we set the app name to be Sunshine.
Then we provide a corresponding package name. For distribution purposes,
the package name should be globally unique across packages installed on Android.
To avoid conflicts, you can use the reverse of the internet domain that you own.
This follows the Java package naming conventions. Note that we're also using
the com dot example namespace for our Sunshine app since it's a sample, but
you cannot publish an app on Google play that uses this namespace.
Usually leaving the default package name that's suggested is fine. However,
this class was originally filmed with an older version of Android Studio which
added the app module at the end of the package name. So go ahead and
make this change now if you want your code to match the rest of the code in
the course. Then we do the project location and
we hit next. Now, we need to choose our minimum SDK version.
In previous versions of Android Studio, you would also have to
select the target SDK. But now the Project Wizard automatically sends the target
SDK to be the latest version. Even though target SDK is already selected for
you, it's important to know the distinction between min and target SDK





09 - Select a Minimum and Target SDK
====================================

Android 1.0 launched in 2008. And in just the six year since then,
there has been 11 new major platform releases. On the Android developer side,
we show the relative number of active Android devices running a given platform
version in this pie chart, because pie charts are awesome. For our purposes
though, you're really better off looking at this as a histogram. If you squint,
you can almost see a vaguely bell-shaped curve, with the oldest releases here at
the left, their popularity dropping off as devices are upgraded or
replaced. The largest proportion of devices are here in the middle,
representing devices about two years old. And the newest platforms,
gaining popularity as new phones are released or upgrades go out,
are here on the right. So with that in mind, the Min SDK acts as your low-pass
filter. Google Play won't show your app on devices running a platform version
lower than its minimum SDK version. So why not just set the Min SDK to one and
support everyone? Generally, you'll want to target as many users as you can, but
there's a cost associated with supporting some of these older versions.
Things like creating different execution paths around deprecated or
update APIs or presenting a different UX to devices with different features. So
you need to balance the opportunity of expanding your audience with the cost of
supporting those new users. Also, remember that each release introduced with it
new APIs and hardware support. So it may not make sense to
make your app available to devices that don't support your minimum feature set.
By comparison, the Target SDK is not a high-pass filter.
It's used only to declare which platform version you've tested on.
An app targeted to a certain API will continue to be forward compatible on
future releases. The platform uses the Target SDK values in case a future
platform makes a significant change to expected behavior. This ensures your app
doesn't break when a user's phone gets upgraded. If you're developing a new app,
there's really no reason to target anything but the latest Android version. And
once your app has been released, make it a point to update your Target SDK and
test as soon as possible when new platform releases roll out so you can take
advantage of every new platform optimization and improvement it has to offer.





10 - Select a Target SDK
========================

The distribution and latest version of Android changes frequently. But
based on this graph, which version of Android should be the target SDK to
provide the best experience on the largest number of devices.





11 - Select a Target SDK Solution
=================================

That's right. While setting Froyo as a minimum SDK, would allow us to support
experience for users on all phones including the newest KitKat releases,
we need to set our target SDK to the latest build, which is KitKat.





12 - Finish Creating a New Project
==================================

Thanks Rado, for telling us about the importance of proper dessert selection.
Let's choose our dessert now. At the current time,
choosing Gingerbread covers 99% of the devices active on the Google Play store.
It's also the cutout for using Google Play services, for things like maps and
location, and the Android support library. The support library package
offers an implementation of UI features that were added in later releases. But
now can be used on older versions of the platform, like Gingerbread.
We pick the latest version of Gingerbread at API level 10, because it contains
bug fixes that API level 9 doesn't have. Then we go ahead and click Next. We're
presented with a list of templates that we can choose from in order to create
our new app. We're going to pick the Blank Activity with Fragment Template.
For background context, an activity serves as the presentation layer for our UI.
And the fragment within it represents a behavior or portion of the screen.
Go ahead and click Next. We call our activity main activity. And the layout
comes from the activity_main XML file. In similar fashion, the Fragments layout
comes from the fragment_main XML file. Then we go ahead and hit Finish.
You should have your newly created Sunshine app with the files on the left here.
Make sure you wait for the Gradel Build to finish. If you still see errors,
you can check the Guide and Instructor notes below on how to troubleshoot.
As a tip, you can check your Min and Target SDK versions in the build out Gradle
file located under the app Source folder. We want the compile SDK version and
the target SDK version to point to the latest table version of the platform.
In this case, it's 19 and not L. Once you have no errors,
we can add a custom app icon that'll be more fitting for
our weather app. Right-click on the app folder and go to New Image Asset.
Download the icon that we provide in the link below, then go ahead and
find that icon. And here you can see a preview of our temporary app icon.
We're going to get a better one in less than five. Then go ahead and
click Next and Finish. And the new icon shows up in the Drawable folders.
Go ahead and try it yourself. Make sure you're using the same settings as we
have by checking the link below. Click here when you're done.





13 - Install HAXM
=================

I know you're excited to run the app. And I'm going to show you how to do it
without a physical device, because we'll be using the emulator. But
first, we need to install one more component from the Android SDK Manager.
Click on this button to open up the Android SDK Manager. Scroll to the bottom of
the list to find HAXM. It stands for hardware accelerated execution manager. And
it will speed up our emulator. So go ahead and deselect the other packages, and
just install this one. Accept the license, and
then continue. We have to do one more step after this to install HAXM.
I'll show you the remaining steps on a Mac, but for the other platforms you can
check the link below. First, find Android Studio under the applications folder,
and then hit show package contents. Then, in the SDK folder,
there'll be an extras folder containing an Intel folder with the HAXM folder.
Then, we click on the DMG file, open it up and
then continue. Step through this installer, and the default values are fine.
Once installation is complete, you can close all the windows.





14 - Launching Sunshine and Creating an AVD
===========================================

Now for the moment you've been waiting for. Let's run the app by clicking on
this button. It will prompt us to choose a device. Without a physical device we
can use the emulator which is included in the Android SDK.
Currently it says that there are no Android virtual devices available. So,
let's open up the AVD manager to create one. Then click New. By creating an AVD,
we can configure an emulated device with the android platform version that we
want to test on, as well as hardware options. For our case,
we're going to emulate a Nexus 5 virtual device, the defaults are fine for now.
So, we're going to hit OK. Then we see that our AVD was created successfully.
We can close the AVD Manager. And when we go back to the Device Chooser dialog,
we'll see that Nexus5 is an option. Then we can hit OK to launch the app.
When it loads up, we see our Sunshine app with the words Hello world.
Keep in mind that the emulator is not just a simulator.
It's actually running the Android Virtual Machine. So you can test and
debug your app on different hardware and software configurations.
You can interact with the app on the emulator using the screen or
with the navigation controls here. You can also invoke other apps,
do network calls, play audio or video and much more. It also includes debug
capabilities, including log output, and the ability to simulate app entraps or
network latency. You can also hit Home and browse around the device to see other
apps. For example, you can check out API demos. Feel free to
play around with the emulator some more. If you open up Android Device Monitor,
you can go into the Emulator Control Tab. There you'll see different options for
simulating behavior on your AVD, such as for telephony and for
location. Now that the Hello world app runs on the emulator,
let's see what goes on behind the scenes to make the app run.





15 - Android Software Stack and Gradle
======================================

The simplicity of hitting run and having your app appear on an emulator,
hides a lot of complexity. Remember that Android is a full software stack.
Adspace is a Linux Kernel,
which handles low level tasks like hardware fibers and panel management.
On top of that, are some core C and C plus plus libraries like Libsc and
SQLite and the Android Runtime. That includes cool Android libraries and
the Android virtual machines, Dalvik or more recently ART. Your apps run
within its own instance of the VM using the classes and services provided here
in the application framework. On top of that, sits the application layer,
which includes your app and every other app that's installed on the device. So,
when you hit Run in Android Studio, the first thing that happens is your code
gets compiled into byte code that can be run in the Android Virtual Machine.
That then gets installed onto the device. In Android Studio,
this is done using gradle, a build tool kit that manages dependencies and
allows you to define custom build logic.
You can manually start a gradle build in the IDE by selecting make project.
You can also do this by going to the build menu and selecting make project from
there, or you can use the gradle console to observe any logs or
build errors, or open the gradle tasks window to see any available tasks.
Double clicking on any of them will execute it. This will work from
the command line too. Once you've navigated to the root of your project folder,
you can run gradlew tasks to see all the tasks that you can run. You can learn
more about gradle by checking out the links in the instructor notes. For now,
note that we start with the project, which gradle then builds and
then packages the byte code along with the external resources such as images,
strings, and uixml into an application package. This is called an APK, and
it's a specially formatted zip file. Once you've got your APK ready to go,
it's signed and then pushed to the device using the Android Debug Bridge or
ADB. If we return to the terminal, you can see that ADB lets you interact and
debug apps on any device, physical or virtual. Things like pushing and
pulling files, viewing logcat output, or
even running a remote shell. So once Android's GDO has ADB installed the APK,
it uses ADB again to launch the app by sending a stock command via the remote
shell, by identifying the package and class name of your main activity.





16 - Debugging with a Physical Device
=====================================

Thanks Veto. And now it's time to install the Sunshine app on our phones.
So to get started, let's grab a USB cable. We'll plug in our device and
then we'll need to enable USB debugging. This can be found in Developer options
in the Settings app. But note that on Android devices 4.2 and above,
the option is actually hidden by default. To find the secret menu,
go to the Settings app. Scroll down to About phone.
Then go down to Build number and
tap on that seven times [SOUND]. Then,
when you go back to Settings, you'll see the Developer options menu appear.
Also, set up your computer to detect your device. Check the instructor notes for
more details. We're going to continue using a device because it's faster and
smoother for development. But if you don't have one,
that's okay. You can continue using the emulator that we created previously.





17 - Launching on  a Device
===========================

Now in Android studio you can hit the play button to run the app on your device.
In the device Chooser dialog. you should see your connected device.
To prevent the dialog from popping up in future times, check this box and
then hit OK to continue. Within a few moments you should see it on your phone.
On the other hand, as Raido was mentioning, you can use command line tools.
From the root folder of the project, use grade lw to build your app on Mac or
Linux. Check the link in the notes below if you're using Windows.
This first command grants execution permission to the gradlw rappers script. And
you only need to do it the first time you're trying to build from the command
line. Then you can call grade lw assemble debug. Once the build is successful,
it creates an APK file at the app/build/apk directory. You can use
adb to install the app. The -r command means that you can replace an existing
version of the app if you've already done it before. For more tips on adb,
you can see the instructor notes below. Then you can use this adb command with
the activity manager tool in order to start the main activity. And there you see
it on the device. And now we see it running in our phones. Woo hoo! Fist bump!
We're on a roll. Let's go find Dan so that we can build up the sunshine UI. Woo!





18 - Start to Build the App
===========================

Let's build up the UI for the weather app. We've created our Android project.
And understand more about basic tools. So let's go to the user interface for
Sunshine. Specifically, we're building the initial screen.
A list of forecasts for the next several days.





19 - Create a User Interface
============================

MainActivity is launched when you start the app. At the bottom of MainActivity,
we have PlaceholderFragment. PlaceholderFragment was generated by
the template we used when creating our project.
A fragment is a modular container within your activity. In later lessons,
we'll look at how to use multiple fragments in a single activity. And
we'll actually explain why, we're using them at all. But for now,
our activity contains just this one fragment. So here in PlaceholderFragment,
is where we reference our UI layout resource, called fragment main.
This XML file lives in the Resource's RES directory of our Project Folder. You
can see other kinds of resources here besides layouts, such as design assets or
drawables, or strings. When our activity runs, it creates this placeholder
fragment which then inflates the XML layout resource, converting everything in
the XML file to a hierarchy of view objects in memory. By holding CTRL or
CMD, depending on your operating system, and clicking on this reference to
fragment main. Android studio will drop us right into the visual layout editor.
Once we're inside a layout XML file, we can switch between the design tab,
where we can drag and drop new UI elements and modify the layout visually. And
the Text tab, where we can see and edit the XML that defined the layout and
UI elements. All of the views we'll talk about here ultimately extend the view
base class. The template we used gives us a relative layout, with some padding
around the edges. We'll get into layout features like padding and margins,
later. Inside our relative layout, is a single TextView that says, hello world.
EditText, is a text entry field that is an editable version of text view.
It has many options, such as whether it supports single or multi line.
There are several styled versions of editText, in the text field section of
android studio. Such as Name, E-mail, Phone, or
Postal Address. Each one sets the soft keyboard into an appropriate entry mode.
ImageView displays the image defined in its source attribute.
It has some really useful features. Like zooming and copying if the source file
is to large or has a different aspect ratio than the image view itself.
A list view is a special kind of view that contains one or
more view that are replicated to display sets of data. In this
case the single textView is used to display the weather information replicated
throughout the list. We'll get into much more detail about list view later on.





20 - UI Element Quiz
====================

The editor is quite powerful. Without using any code, one can create relatively
sophisticated layouts. Make sure to use the built-in styled text widgets for
large and medium text. All of the text is just placeholder text, so
you easily enter it using the visual editor. If you're proud of your creation,
take a screenshot and post it along with the XML into the forums.
Now that you have some background on the basic tools in Android studio, and
a few of the basic android user interface views,
Katherine will show you how to start making some sunshine.





21 - Add ListItem XML
=====================

Perfect, thanks Dan. Since he's turned you into a UI expert now, where should we
define this list item layout? If you're thinking the layout folder,
under the resources directory, then you are correct. Here you can create a new
layout resource file called list item forecast. Inside that file,
define a text view as the root view, then assign it an ID list item forecast
text view. Give it a minimum height, so that the list item isn't too short for
tapping on and vertically center the text within that item. Go ahead and
try it yourself now. And check the box when you're done.





22 - Add ListItem XML
=====================

And here's the solution. You create a new layout resource file. Call it list
item forecast. And the root element is a TextView. Then you just hit OK.
Now that we have the item created, we can switch to the Text pane.
Here's the solution code. We have a single TextView where the width is
match parent and the height is wrap content. We also give it a minimum height.
So the item is tappable. We use the framework preferred item height for this.
Since there'll be more vertical space now with the minimum height, we specify
gravity so that the text inside the text view will be centered vertically.
Lastly, we specify ID, which we gave you earlier. Great. Now we have a list item





23 - Introducing Responsive Design
==================================

Imagine spending hours, crafting the perfect layout. What happens to
that layout, when you rotate the device, into a landscape orientation?
Or when it's run on a device, with a much larger screen? The good news,
is that Android provides excellent tools to help you with layouts.
You'll want to understand them to make your app look amazing across the wide
variety of phones, tablets and other devices you'll want your app to work on.





24 - Why AbsoluteLayout Is Evil
===============================

Building something that looks great in the visual layout editor is the easy
part. You also need to consider different screens and orientations. As you
begin to layout your UI, there's a temptation to build it pixel perfect for
the device you happen to be using at the time. This is reinforced by
the fact that your apps window doesn't generally change size while it's running.
So you can't just grab the lower i-corner and grow and
shrink your app to see how it behaves. But if you've ever developed for
the web or desktop, you know this static approach is a bad idea.
When Android first launched, HVGA 480 by 320 resolution screens were standard.
As we began exploring how to build engaging user experiences on those devices,
we could use absolute layouts to define the exact location of each
screen element. But within a year, the first WVGA Android phones were released.
And now Android runs on everything from phones to phablets, tablets, TVs and
wearables with any screen size, resolution and aspect ratio you can imagine.
So, just like desktop or web where you might use panels or
CSS, your Android UI needs to scale based on the screen it's running within,
which is why absolute layout was deprecated.
[BLANK_AUDIO]
In favor of layouts like LinearLayout, RelativeLayout and
GridLayout, they can dynamically resize and
adapt to any screen, following the principles of responsive design.





25 - Responsive Design Thinking
===============================

Responsive design, I know. [LAUGH] It's fun to resize your web browser and
see which sites resize gratefully and which sites remain defiantly large with
gross horizontal scroll bars. But responsive design is not just for
the web. Today, the lines between phones and tablets are disappearing. So
it's important to think about your UI will scale in our multiscreen world.
Don't be overwhelmed. Just like designing responsive layouts for the web,
build your layouts to be reasonably flexible or within a common device size.
Then you can set break points, providing alternative layouts for
those various sizes. Think about it this way,
small phone, large phone, medium tablet, and large tablet.





26 - Layout Managers
====================

Going back to what Rato said, frame layout, linear layout, and relative layout
are three of the most common layouts you will use in building out your UI.
These all descend from the view group class, designed to contain and
give order to child views. They each have their strengths and
you should always try to use this simplest layout that will get the job done.
Frame Layout is great for simple layouts when you only have one child to view,
like a list view that fills the entire content area. Linear layout is
perfect for stacking the views vertically or horizontally, one after another,
it is also the only way to break up the display proportionately. Relative layout
is powerful but a bit more complicated compared to the others. Throw a bunch of
views inside a relative layout, and then you can configure each views
position relative to parent, the relative layout, or to sibling views, tons of
possibilities. We'll explore these layouts in greater detail when we build more
complex screens. For now, let's get back to building our forecast list.





27 - ScrollViews vs ListViews
=============================

On the surface, creating a list of items is simple enough.
Android includes a Scroll View into which you can place any
linear layout that in turn arranges each item it contains into a vertical list.
Note the items which have fallen off the bottom off the linear layout. And
therefore aren't currently visible in that UI. The Scroll View,
as the name suggests, will let the user scroll through the contents of
the layout it contains. But there's a challenge associated with that approach on
a device with limited memory, and where touch responsiveness is
critically important. If you have 50 items in a list and
can fit 10 items on screen at any given time. What's the minimum number of
views you'd need to create in order to scroll through every item on the list?
The answer might not be immediately obvious but think about ways in which you
can be more efficient in your use of the views used to display the entire list.





28 - Scroll Views vs ListViews
==============================

Adding every item to the linear layout within the scroll view,
meant that every view we create, sticks around, taking up memory, even if
it's never been seen. We want to try and create a way, that we only need to use,
as many views as are currently visible in the screen plus one on either end,
to make sure we can scroll without flickering. To do that,
Android uses List View. So, let's take a closer look at that now.





29 - ListView  Recycling
========================

ListView starts by requesting a view for every visible item, however many you
can fit onto the screen. It'll also create a couple in either direction to make
sure we can scroll without seeing a flicker as a new view is created and
populated. Then it creates new items just in time. So it's next in line to
be visible to the user. So if the user never scrolls to the bottom of the list,
the ListView will never request that view from the adapter. But this is really
just a half measure. As you can see, if the user keeps scrolling, we could
potentially just keep adding new views, even if they disappear off the top of
the screen. Eventually that's going to lead to the same impact in memory use and
performance as if we had just created all of these views directly at
the beginning. The solution is recycling each view as it scrolls off the screen,
allowing it to be reused when we need to show another item as it
moves into view at the top or bottom. So rather than having to create and
then hold in memory each item of the list as it comes in to view,
we only need to do the creation step for the number of visible items and
couple on either side. Then whenever a new list item comes into view,
we just update the data displayed in one of our items in our recycle bin.
The result? Less memory overhead, smoother scrolling and
less view management you have to do yourself. This same recycling behavior is
implemented across all AdapterView descended classes, such as GridView and
ListView, which also introduces the reason that the adapter isn't built
directly into these controls themself. By keeping them separate,
your adapter defines how to display each element of the underlying data,
while the adapter view implementation itself is responsible for
controlling how each of these elements is laid out. Be it a list or
a grid in these particular instances.





30 - Add ListView to layout
===========================

Thanks, Rado. Now that we know the distinct advantages of using a list view,
we can use a list view to display the list of weather forecasts in our app.
Now, which file in the layout folder should we modify to accomplish this?
Well, we want to add the list view directly to the fragment. If you open up
the fragment_main.xml file, you'll notice that the layout includes a relative
layout as the parent view, as well as a child TextView. Modify this file so that
we show a ListView instead of a TextView and assign it an ID ListViewForecast so
we can reference it later. Also, since this layout only contains one child's
view, it would be more efficient to actually switch to a FrameLayout instead of
a RelativeLayout. After you make the changes, compile and
run your app. You'll be a little underwhelmed with the blank screen, but
that's okay. That's because we haven't populated the list view with data yet,
and that will come in a later step. Check the box when you're done.





31 - Add ListView to layout
===========================

You should have changed the TextView into a ListView element.
Then you can remove this text attribute because you don't need it anymore.
Now from the UX mocks, you'll remember that the list takes up the whole screen,
so we want to set the width and height to be match_parent. That will make
the ListView match the dimensions of its parent, which is this RelativeLayout.
And to find out the parent of this RelativeLayout, it's actually in this
activity_main XML file because the fragment is contained within the activity.
Within this file, we see that this view is also match_parent for height and
width. So now we can confirm that the ListView actually will take up
the full screen. Going back to the fragment_main file, we can add the id,
listview_forecast, onto the ListView. And then, since this layout only contains
one child, we can simplify the layout by changing this into a FrameLayout.





32 - Create Some Fake Data
==========================

With our ListView ready to go, we'll want to create some fake data to
populate it. Open up MainActivity.java and then scroll down to the bottom where
the PlaceholderFragment class is. Within the onCreateView method,
create an array list of strings to represent the weather forecast list items
shown in the wire frames. For example, the Today List item will be
represented as a string shown here. When you're done, check the box and submit.





33 - Create Some Fake Data
==========================

At this point, we should have created some fake data to display in our list.
We created an array of stings to represent each weather forecast item and
then returned it into an array list.





34 - Adapters
=============

Given some sample data how do we populate a ListView?
Let's look at an example of some contact data and how it can be displayed in
the list. We start with our raw data of contacts which is three contacts,
as well as our profile images. Then we pass this data into the adapter so
it has a reference to it. The adapter also knows how to build a list item layout
for each of these data items. So, it could go ahead and create the layouts for
them. But we don't need to create the views yet until the ListView requests that
it needs them. For example, if you had hundreds of contacts here,
you wouldn't want hundreds of layouts sitting around that aren't being used.
Then when you bind the adapter to the ListView,
the ListView will ask how many items are actually in the data set.
And the adapter will check in the data set there's three items, so
we will return that to the ListView. Now the ListView knows that it will have to
populate itself with three list items. Now the ListView starts at position
zero and asks for the list item layout located at that position. It goes back to
the adapter and the adapter checks that at position zero, we have the contact,
Anna. The adapter knows how to create a list item layout from the contact, Anna.
So, it goes ahead and does that. And then we return it to the ListView. And now
we see that the Anna list item is located at the zeroth position in the list.
Next the ListView wants to get the item at position one, so
ask for the layout from the adapter. The adapter checks that at position one,
we have the contact Bob. The adapter knows how to create a list item layout for
Bob so it goes ahead and does that and then it returns it to the ListView. And
now we see that the list item for Bob has been successfully added
to the ListView at position one. Now, the ListView fetches its
last item because it knows that there was three list items to expect.
It fetches the last list item layout at position two by asking the adapter.
The adapter checks that at position two, we have Charlie, as well as that image.
And we know how to make a list item layout for Charlie, so we can go ahead and
do that, and return it to the ListView. And now we have the list item for
Charlie and position two in the list. And we have our complete list





35 - Initialize the Adapter
===========================

In our code, we're going to initialize the adapter within the placeholder
fragment on create view method. Because that's where we want the list view to
be populated with data. We're using a array adapter, and we can initialize it
with four parameters. A context, the ID of a list item layout,
the ID of a text view within that list item layout, as well as a list of data.
We'll go through each of these parameters one by one now. First, for
context. It contained global information about the App environment.
It allows us to access system services and resources, as well as the application
specific resources that we've defined. We use the fragments containing activity
as the context. So, we're going to call getActivity for our parameter here.
Since the adapter needs to know the layout for each list item, and
needs some reference to the XML layouts that we've defined. The r.java class is
a generated file that creates human readable identifiers for our resources. For
the list item layout, we refer to it in code as r.layout.list_item forecast.
This was the name of the XML file that we created earlier. Next,
the array adapter needs to know how to take the weather forecast string and
set that to be displayed in a text view. So you pass in the ID of the text view
that we defined earlier, in the list item layout. Even though these two resource
IDs look similar, one starts with R.layout while the other starts with R.id.
R.layout refers to a layout file, while an ID refers to a specific [UNKNOWN]
element with the matching ID attribute. And lastly, we pass in a week forecast,
which is the array list of forecast data that we defined earlier.
Now go ahead and initialize your adapter. Check this box when you're done.





36 - Initialize the Adapter Solution
====================================

When you've declared your array adapter of strings, it should look something
like this. We pass in a context, which is this fragment's parent activity,
as well as the ID of the list item layout, and the ID of the text view we
want to populate. And lastly, we pass in the forecast data.





37 - Finding Views findViewById
===============================

Once the adapter is initialized, let's bind it to the list view. But
you may notice that we don't have a reference to the list view in our fragment.
It was only defined in the layout XML. The system takes and
inflates layout XML files. And
turns them into a full view hierarchy with a root layout of the main activity at
the very top of the view tree. We can also assign IDs to each view in the tree,
but it's not required if you don't need a reference to an individual view.
For example, this image view doesn't have an ID associated with it and
that's okay. Within the Java code of the associated activity or
fragment. If we need a reference to the button, we can simply call findViewById,
which will traverse down the hierarchy until it finds a view with the ID button.
And then it will return that. Similarly, we can traverse down the view
hierarchy to find the linear layout with the ID container, and then return that.
We could do the same to find a reference to this TextView by traversing down
the view hierarchy. But you'll notice that we already have a reference to
the container which is a direct parent of this TextView. Hence, we can just call
container.findViewById to search this subtree to find the TextView with this ID,
and then return it. With this method, we have a smaller sub-tree to search for
a given view, as opposed to searching the entire view hierarchy.
In the Android Java doc for the View class, which is linked below if you want to
follow along, it contains an example where the button with Define in Layout XML.
It was assigned an ID My Button. Then in the Activity, we can use the find
view by ID method, with that ID, so that we can get a reference to the button.
Then we can change it dynamically, such as by adding a click listener to it.
Now in the placeholder fragment class, bind the adapter to the list view.
Luckily, we did assign an ID to the list view earlier. So, we can find it
easily now. Think about the smallest sub-tree in the view hierarchy that you
can call find view by ID on. Then set the adapter on it. If you want an example,
you can see one in the link below. Check the boxes when you're done.





38 - Finding Views findViewById
===============================

Here's the solution for
binding an adapter to a ListView within the PlaceholderFragment class. First,
we find the ListView in the view hierarchy by using the findViewById call and
then, we set the adapter to it. The adapter will supply list item layouts to
the ListView based on the weekForecast data. Note that the rootView here
refers to the root view of the fragment, which we just inflated up above here.





39 - Great Work
===============

And now we have this beautiful list of weather data in our app. Great job.
High five. [SOUND]





40 - Lesson One Recap
=====================

You accomplished a lot in this first lesson. Starting with nothing more
than Android Studio and hope, you've created your first project. Built and
deployed it to virtual and actual devices, and
even created a simple list-based UI that's populated using an adapter.
You also learned a little about what makes mobile and Android, in particular,
a different environment to develop for the desktop, web, and server. But we've
only just gotten started. The app we've got so far is using static mock data.
So join us in lesson two to learn how to hook it up to real weather data.
But before that, take a moment to cleanse your palate with
a brief interlude exploring the history and evolution of the Android ecosystem.





41 - Storytime Android Platform
===============================

I asked my first Android question on Stack Overflow, on August 25, 2008.
Was about a problem I was having on the 0.9 Android Beta SDK.
Android has evolved quickly since then, and
no where is there more power than here, in the Android Dessert Sculpture garden,
at the very heart of Android, building 44 at Google HQ.
More than just a convenient place to take a photo with a life-size sculpture of
your favorite dessert, it also tells the history of Android. In terms of food,
every Android launch has two things in common. Work from work, Bacon Sundays,
and the crunch time leading up to each launch. And a dessert themed code named
memorialized forever here, in the Android Dessert Sculpture garden.
[NOISE] Each release brings with it new hardware, APIs, and features for
developers to play with. The third major Android release, 1.5 or Cupcake,
as it was code named, brought video recording and widgets to the Android SDK,
and the first baked good to our front lawn. Android 1.6 and 2.0 brought with
them giant donuts and eclairs as well as support for different screen sizes, and
tracking multiple simultaneous touch points. The biggest challenge for
Android 2.2 was coming up with a baked dessert starting with the letter F.
A challenge we ultimately ignored,
instead introducing this giant bowl of froyo to the neighborhood.
Along with that, in cloud's device messaging to the platform.
[NOISE] That brings us to Android 2.3, and this giant life-size gingerbread man,
along with system support for barometers, gyroscopes and NFC.
Android 3.0 was the Honeycomb release. The first one targeted, specifically for
tablets and paving the way for Android 4.0. Android 4,
Ice Cream Sandwich brought phones and tablets back together. It also introduced
Android Beam, and everyone's favorite font, roboto. Android 4.1 Jelly Bean
was the first platform release to feature rich expandable notifications.
It's also the first sculpture to spontaneously combust thanks to a version one
design, that we've since patented as a solar powered pressure cooker. But
that's another story. [NOISE] Which brings us to the newest member of our team,
Android 4.4, Kit-Kat. With sensor batching, host card emulation and an API for
text messaging. So how do the dessert names get decided? Well ultimately,
the only real rule is sequential lettering. Then it's down to intense lobbying,
threats, bribery and [UNKNOWN] on the ease of spelling, cultural resonance and
statuability of each proposal. Eventually something just sort of sticks.
Even then, we can surprise ourselves. Take a look at the GIT history for
the K release, and you'll see that even KitKat wasn't always KitKat. As for
what comes next, we'll have to wait and
see what the next statue is, before we know for sure.
