
01 - Intro to Lesson 6
======================

Now that we've finished build the UI for our app,
it's time to think about how it should operate when it's
not in the foreground. One of the most powerful features of
Android, is the ability for any app to run in the
background. But with great power comes great responsibility. So, you need
to be very conscious of how it's consuming resources. In this
lesson, you'll learn more about how the Android framework manages background
apps and you'll be introduced to the service class to help
make that happen. You'll also learn techniques for efficient
data transfers using sync adapters and Google Cloud messaging. And
be introduced to the notification framework to send messages
to your users when your app isn't in the foreground.


02 - Sunshine in the Background
===============================

Remember back in lesson two when Katherine used a
AsyncTask hooked up to a refresh button to update
our data? Rato talked about how that was a
bad idea, because the AsyncTask is not tied to
the activity life cycle. The virtual machine will hold
on to the activity object as long as the
AsyncTask is running, even after Android has called onDestroy
for the activity and expect it to be discarded.
If you rotate your phone, the behavior is to
destroy your activity and instantiate a new one. The
naive AsyncTask implementation now has two threads trying to
perform the same update, and so forth. The point is,
it's not the best pattern for a potentially very
long background operation, such as fetching from web services. If
you leave the app, the asynctask will continue to
run for as long as your process is kept alive,
but will run at a low priority, and your process
will be the first thing to be killed if the device
needs more resources. And there's a bigger problem. Your app
has to be visible and running in the foreground to instantiate
the task in the first place. Because we started a
task, to update the weather when we started the app, this
can have undesirable behavior if the weather changes rapidly. So
now we're going to learn the right way to perform updates.
We'll want to automate the process while the app is in
the foreground. But even more importantly, we want the app to get
regular updates in the background with minimal battery drain. That will
be especially important later in this
lesson when we introduce weather notifications.


03 - App Lifecycle and Services
===============================

Back in lesson four, we learned that the
Android Runtime will kill apps with no visible activities,
in order to free resources needed by the foreground
app. But what if your app has tasks that
need to continue when the activity isn't visible, things
like downloading files, uploading photos, or playing music? Well,
there's an application component for that. Services. We've already
introduced Activities, Content Providers,
Broadcast Receivers and Intents. Services,
are the final piece to the Android app component
puzzle. You start services much like you do activities, by
passing in an intent to a Start Service call.
And you can stop services the same way, by calling
Stop Service and passing in the name of the
service you want to stop. Unlike activities, services have no
user interface and they run at a higher priority than
background activities. This means that an app with a running
service is less likely to be killed by the run
time, in order to free resources for the foreground activities.
In fact, by default, the system will attempt to restart
services that are terminated before they are stopped from within
the app. This is reflected in this simplified life cycle.
Compared to Activities, Services are designed to execute longer running
tasks that shouldn't be interrupted. Typically, you'll only need to
override the onStartCommad handler, which is where you begin the background
task you wish to execute. But notice that there are
no handlers for monitoring changes in state, to reflect the
app moving to the background. This is because the running
service itself sends a signal to the framework that the
containing app should be considered higher priority than other apps
in the background that don't have running services. In some
cases your service may performing a task, that while not
having UI, can't be interrupted without interfering with the user experience.
For example, playing music or helping with in car
navigation. In these cases, you can indicate that your
server should be considered to be running in the
foreground by calling startForeground. You'll notice that this call takes
in a notification. This will be displayed, and can't
be dismissed until the service has stopped, or you
call stopForeground. You'll learn more about notifications a little
later, with Dan. But for now, note that a foreground
service runs at the same priority as a foreground
activity. Making it nearly impossible for the run time
to kill in order to free resources. Now, you
may be thinking to yourself, I could save a lot
of trouble dealing with life cycles, just by creating
long running, or even foreground services. Well, I grew up
along the coast in Australia, so I learned young
that swimming against the current is exhausting and ultimately futile.
In this case, that means making it more difficult
for the system to manage resources, ultimately leading to a
worse user experience. Swim with the current. Use foreground
services only when and for as long as absolutely necessary,
and stop all services as quickly as possible. It's
also important to note that like activities and receivers, services
run on the main thread. So you'll need to
use a background thread or a think task to execute
the long running tasks you wish to do
within your service. To make life easier, you can
use the intent service class. Which implements the most
common best practice pattern, for using intents, which are
executed within a service. It creates a queue of
incoming intents, passed in when start service is called.
These are then, processed sequentially on a background thread,
within the onHandleIntent handler, within your intent service implementation.
When the queue is empty the service self terminates
until a new intent is received and the process begins
again. Services are a powerful tool and it's important
to understand how you can use them but in practice
there's often a framework alternative to rolling your own
service implementation. Whether that be an intent service for executing
background tasks or the sync adapter which you'll learn about
later in this lesson. Perfect for performing background data synchronization


04 - Application Priority
=========================

So now that we understand how services work,
let's review how Android determines your app's priority. App
priority is divided into three general buckets. Critical,
high, and low. Within each bucket the apps are
prioritized in a queue. With the app that's
been at the lowest priority for the longest, the
first to be executed. Critical apps are those that
are active. They're in the foreground, interacting with users.
That includes activities in the foreground and apps
running foreground services. High priority apps, include any visible
activities and any application with a running service.
While less impactful than killing a foreground app, destroying
visible activities or cancelling services running tasks like
background updates it's still going to be noticeable to users.
So, the system will only kill them in
an extreme resource crunch. Apps in the background though,
they're the red shirted ensigns of the app priority
landing party. Any background app will be killed as
needed, in a last seen, first killed order, in
order to help support the higher priority apps. I like
to think of this as the three laws of
Android resource management. Law one, Android will keep all apps
that interact with the user running smoothly. Android will
keep all apps with visible activities or running services running.
Unless doing so violates the first law. And third, Android will keep all
apps in the background running, unless this
violates the first or second laws. So
with all of that in mind, consider these four apps. What do you
think is the priority order of each of these apps, according to the system.


05 - Application Priority
=========================

That's right. Maps isn't visible or running any
services, so it's the most likely to be killed.
G-mail is running a service, but it's not interacting
directly with the user while Google Music and the
camera app are. Of those two, the Music app
must have been in the foreground for longer than
the camera. So it's got a slightly lower priority,
though neither are in any danger of being killed.


06 - Using Services
===================

So, how would we use a service to
implement our application? Fortunately, we've already done most
of the work. We have a content provider
with a content notifier that will notify our
content observers. Our FetchWeatherTask already runs completely independently
of our UI. Good on us. Now, we
can make use of that Intent Service that
Rato mentioned earlier. With just a few small
changes, we can get our code working,
with an Intent Service, instead of with our
Fetch Weather task. To start, let's create a
new package for our service. [SOUND] Then we'll
add a new Java class in that package that extends inside Service. We hit Ctrl+I
to once again, add the required abstract method.
And Ctrl+O in order to add the constructor.
Since the service is an Android component, you guessed it, it needs to be in the
manifest. Okay, let's do this. Let's finish implementing the SunshineService
and call it from ForecastFragment. You can start a service using an intent with
the StartService method. It will help to remember how to use explicit intents.


07 - Using Services
===================

All right, you're done. Let's talk about how I
solved this one. First, let's take all our code, and
copy it from the fetch weather task. The intent
service actually creates a helper thread for us to run
on. Similar to what async task does. So we
can just copy this stuff from doing background to on
handle intent. Let's add a few helpful constants, such as
log tag and an Intent Extra, so we can pass
in the location query. Now, we're just going to
go through and clean up some of the errors.
After all, Intent Service doesn't return a value. And
since it's a service, it has its own context.
And that's it. We've turned out fetch weather task
into an intents service, pretty straightforward. Now we just
need to call it. So, now in the update
weather function from within forecast fragment, we can call
the service using an explicit intent,
putting the parameter into an intent extra.
All right, let's take a look at how that runs. And when we
hit the refresh button, it'll use our new service. Pretty nice. And we
really can't tell any difference, which is how we'd expect things to be.


08 - Using Alarms
=================

So, now, we have a simple service. Wasn't that easy?
But it still doesn't wake itself up. Fortunately, there's a system
service for that. This is a good opportunity to introduce
the AlarmManager. The AlarmManager allows you to tell the system that
you want it to wake a component of your application
up after a period of time and do some processing in
the background. You can even have it wake up your
application periodically but, what do we wake up in the background?
That would be an Android component we haven't
seen before called a Broadcast Receiver. A Broadcast Receiver
is a special class, that is used to
receive intent broadcast often from other applications. Typically a
broadcast receiver will register an intent filter for
these broadcasts. It's also one way the application will
listen in on alarms. So let's add some alarm
stuff. First, I'm going to add a Broadcast Receiver
as a static inner class of Sunshine Service.
Since this is an Android component, I'll register
this Broadcast Receiver in the manifest. Note the
way a static inner class is notated. Okay, I've
given you the bones of a broadcast receiver
that can handle an alarm, but now it's
your turn. You can create a PendingIntent from
an explicit intent to have the alarm manager activate
your broadcast receiver. I recommend setting the alarm to something absurdly
short, like five seconds, so you can easily test that it's working.


09 - Using Alarms
=================

All right, you're done. Let's look at the solution.
As I said in the question, we're going to be
working inside a forecast fragment in the update weather function.
First, we're going to need to create a standard intent
for our alarm receiver. We then add our location query
as an extra. We then wrap that in a pending
intent. A pending intent is a special kind of object
that describes an intent. This allows other applications to implement
the feature of the original intent that's used to
create the pending intent. We're only going to use this
pending intent once, so I set flag one shot.
Then, we get the alarm service and set the alarm
to trigger five seconds from now. But we still
need for our alarm to do something. Back in the
Broadcast Receiver, we need to send the standard intent
to start our service, and that's it. Let's try running
the app. All right, so now we've got the
Alarm Manager getting in the way of our service. We
hit refresh. It'll take us about five more seconds
before we actually see the data now. And there we
have it. A very, very simple alarm. Even with
this updating in the background potentially, and using a service,
we could still be more efficient in our use
of phone resources. Rato can tell us more about that.


10 - Transferring Data Efficiently
==================================

Typically when we talk about data transfer efficiency, we're talking
about limiting the amount of data being transferred. That reduces
the bandwidth and the cost and is generally a pretty
good idea. But on mobile there's an extra factor. As you
can see, it turns out that the cell radio is
one of the biggest battery drainers on the device. So making
your data transfers more efficient. Will also result in improved
battery life. Now, the basic level being more efficient means spending
less time transferring less data, reducing your payloads, and updating
less often can take you some of the way there,
but then what? Can timing make a difference? It turns
out that it can, and it's a situation I like
to think of as the cookie Droid conundrum. Do we
perform fewer downloads with larger payloads? Or do we perform
lots of small transfers just in time for when we
need them? One big cookie or lots of little cookies?
So, what do you think? Should we have
a smaller number of large downloads illustrated by
this big cookie? Or a large number of
small downloads, as illustrated by lots of little cookies.


11 - Transferring Data Efficiently
==================================

Now, at first glass, reducing the payload of each transfer, and
only transmitting data when it's required seems like a sound approach.
You're reducing the amount of data being transferred, so that's less
data on the network. That's less superfluous work being done storing processing
data on the device. It's basically a case of putting off
any work, until you actually know you need to do it.
But it turns out that this approach has it's drawbacks, compared
to the big cookie model of all of that work up front.
So, overall, this is a better solution. But let's take a closer look at the big
cookie model. And to do that, we really
need to understand the underlying cell radio state machine.


12 - The Cell Radio
===================

The cell radio in your device operates roughly like this.
From an initial idle state, it takes a couple of seconds
to turn on until it can start transmitting. That kind of
latency makes for a sucky web browsing experience. So rather than
going back to idle, state machine stays on at full power
for a certain amount of time. Typically, around five to ten
seconds before dropping to an intermediate low power mode that uses
less battery than full power, and has lower latency to return
to full power than the standby mode. If a new
transfer is initiated, the radio will be promoted back to
full power mode. And if nothing happens for another period
of time, typically around 30 seconds to a minute, it'll
drop back to standby. The exact latency in tail times
varies between carriers, and even in carriers between states and
countries, as they try to balance low latency with longer
battery life based on factors like cell congestion and typical
prevailing network conditions. So the exact timings vary. How
do we optimize our transfer frequency? Ultimately, it doesn't matter
what the specific timings are. You just need to
understand that the network is going to attempt to balance
low latency with high battery life. For us, when
it comes to planning out data transfers, we really like
to be somewhere around here,. Now, if we return
briefly to the cell radio state machine, we know that
every time we perform a data transfer, the radio will
stay active for at least another five seconds of full tail
time, and anywhere from 30 seconds to a minute at low
power before it finally returns to standby. That means every time
you initiate a transfer, you're powering the cell radio up for
at least 20 seconds. So let's take a look at how
that affects an app, using the little cookie approach. An app
like this can drain the battery without even having to transfer
much data. Each of these small peaks is
an app pinging its analytics back to the server,.
In this case, every 15 seconds. These logi-peaks represent
intermittent data transfers based on user interaction. For example,
they may be viewing a restaurant listing or looking
at tomorrow's weather forecast. Beneath it, we've graphed how
this affects the radio state. The blue shows active
data transfers. The red, the radio in full power.
And yellow showing low power mode. The gaps in between, if
there were any, indicate when the radio was idle. So while
this app is running, what is the percentage of time that
the cell radio is able to go back to its idle state?


13 - The Cell Radio
===================

That's right, whenever this app is running, it's
keeping the cell radio powered on continuously. In
fact, the radio updates alone are enough to
prevent the radio from ever returning to idle.


14 - Big Cookie Model
=====================

In this app, we see an example of a
defragmented network traffic that uses the big cookie model. All
the repeating transfers have been bundled together, and all
the intermittent transfers have been
largely replaced with aggressive prefetching.
Obviously, you usually can't entirely predict what data users
might need, nor can you ignore either client or service
site changes the need to be synchronized. You can
aim to minimize the number of radio state transitions through
a combination of aggressive prefetching in addition to batching
and queueing any transfers that aren't time critical and
bundling these with user initiated time critical transfers, or
those initiated from the server. If we compare the impact
on the radio of the big cookie model compared
to the previous on demand approach, you can see it's
now idle nearly two thirds of the time. Even
the active radio percentage has significantly dropped, thanks to improved
download efficiency as a result of transmitting more data in one shot


15 - Data Transfer Best Practices
=================================

The most important thing you have to remember is that
every time you transfer data, no matter how small, the radio
could stay powered up for nearly half a minute. So every
decision you make will be based on minimizing the number of
times that this happens. But of course there's a balance here.
You want to download all the data a user is likely
to need for the current section in a single burst over
a single connection at full capacity. But of course, you don't
just want to pull down everything wasting battery power and bandwidth
downloading data that's never going to be used. Now I could go
on for hours on this topic, but Dan's getting impatient. And
you can learn the details on how to implement each of
these best practices, including pre-fetching,
batching and bundling, burying your
update frequency, and minimizing your payloads by watching the series of
Dev Lite videos or reading the developer guides linked to in
the instructor notes below. Now before I leave it to Dan
to show you how to implement a sync adapter for
Sunshine that takes advantage of a lot of the best practices
I just described, let's consider what the best practice would be
if you were building something like a news reader app. How
much data should you download when the app is first started?
Just the front page of headlines, all the stories and images
linked to from that front page? Every story available, but none
of the images? Or every story and every image currently available?


16 - Data Transfer Best Practices
=================================

Pulling down all the data will reduce latency and maximize the
battery efficiency. But it's unlikely the user is going to read every
article. So you'd just be wasting a lot of bandwidth. Downloading
all the articles without pictures actually doesn't improve our efficiency if
you're just downloading the front page, because you'll still need to
activate the radio to download those pictures whenever an article is
selected. The right answer is to get the headlines and the
articles linked to from that front page, 'cause they're the most likely
to be read as the user starts
browsing through the app. You can then incrementally
download more articles each time the user moves
into an area you haven't pre-fetched for yet.


17 - Introducing SyncAdapters
=============================

There's a lot to learn with making background
transactions efficient. But the good news is that Android
gives you the Sync Manager framework that implements
many of these best practices. You utilize that framework
by implementing a sync adapter. The framework, originally
introduced in Android 2.0 Eclair or Android API level
basic framework that Google apps use for efficient synchronization.
Ultimately, it's a centralized place to put all
of the device data transfers in one place.
So that they all be scheduled intelligently by
the OS. In other words, that's one big cookie.
Android Sync Manager handles synchronization requests using sync
adapters. The Sync Manager batches and time shifts
these requests when possible to allow your data
transfers to be scheduled with transfers from other apps,
all working towards the goal of reducing the
number of times the system has to switch on
the radio. If your device has less memory,
it will schedule fewer simultaneous synchs. The Synch Manager
also takes care of things like checking for
network connectivity before initiating transfers and retrying downloads when
connectivity is dropped. The synchronization framework works with content
providers for two way synchronization and leverages the Android
account manager to provide synchronization services that are
tied to accounts. Our application will do neither
of these things, but we'll still have to
deal with some of the complexity of these features.
This can make SyncAdapter seem daunting at first.
What does the SyncManager do to help you fetch
data from the network? Does it schedule your
network jobs with other apps, implement a synchronization protocol,
store account information, or has logic to retry
your request? Select all of these that match.


18 - Introducing SyncAdapters
=============================

The SyncManager does schedule your SyncAdapter
jobs. But they don't have anything
to do with what goes over
the wire. There's no standard synchronization protocol.
And while they're to tied to AccountManager, they've nothing to do with storing
account information. However, they will auto
retry requests, if network conditions are spotty.


19 - Implementing a SyncAdapter
===============================

Let's take a look at what it will
take to implement a very basic SyncAdapter. We're going
to write two services. Each service serves the primary
purpose of delivering an object that represents an Android
Binder interface to one of the system frameworks.
A Binder is actually the low-level glue that implements
cross process communication in Android. You've been using Binders
every time you talk to an Android system service.
Intents and content providers are just high-level abstractions on
top of the Binder interface. There's a whole language known
as AIDL to help define these interfaces. We're not
going to cover all this here, but there's a lot
more you can do with services and Binders. One
more thing before we start, we're going to define an
Authenticator Service and an Authenticator. But it will only
be used by the Android accounts framework to allow us
to create an account. SyncAdaptor's need an account.
And the account framework requires that there be
an authenticator delivered by an authenticator Service. You'll
see that our authenticator is just a series
of stubs, with exceptions that get thrown for
each call just to prove that it isn't
really used. One final note. This section approximately
follows the online training at developer.android.com around sync adapters.
Feel free to look there if you have
any more questions. We're going to create a new
package, sync, to house all of this goodness.
And a new class file for our authenticator. This
code that we're about to paste in really
just comes from the developer.android.com website, and as I
mentioned earlier it's just a stub. You can tell
because we throw exceptions for calling any of the
functions except for the constructor. And one
more file. Create SunshineAuthenticatorService. This is more code
that is written for us. It allows
the account manager to access the empty authenticator
that we just pasted in. Now we add the account type in our strings.xml. The
account type string suggests that it is specific
to our app. If we had many applications
using the same account, we might want
to create just an example.com account. We'll also
begin to clean things up and add
a content authority string. Note that this matches
our content provider string. We'll fix the XML file later so they both use
this same string. We create a new XML resource file, filename
authenticator.xml, with root element account-authenticator.
And you probably noticed that SunshineAuthenticatorService is
actually a service that needs to be registered
with a package manager in AndroidManifest.xml. Here's
some more pasty goodness that does just that.
Now be very, very careful. These strings
all have to match precisely. The error messages
that the system gives for having incorrect accounts
are not necessarily intuitive. And with that you
should be able to create valid accounts. Once again,
this is all just so that the SyncAdapter can be
tied to an account. You don't actually use this
at all. All right, let's tweak our provider tag in
the manifest. We're going to add the syncable attribute.
This just lets Android know that we're planning to synchronize
the content provider with the server. Also, we'll set
android:exported equals false. We had it at the default setting.
Which means that other apps could see our content. Finally,
let's change the authority to use our new string. Now
for some additional permissions. We need to be able to
read and write sync settings. That makes sense. We also
have to authenticate accounts, even though we're not really using
them for anything. None of these permissions are ones users
should be concerned about. But as developers, we always want
to be careful when we have to add new permissions.
Let's create the SunshineSyncAdapter file itself inside
of sync, which extends the abstract threaded sync
adapter class. Hit Ctrl+I and then Ctrl+O
to implement the necessary abstract methods in constructor.
We'll use the first constructor. We'll fill
this out later. As you may recall, the
sync adaptor pattern requires yet another service. So
we're going to create another Java class called SunshineSyncService.
This class is used to deliver the sync adapter Binder to the sync manager.
The Binder is implemented for us by
the abstract threaded sync adapter class. And returned
in the getSyncAdapterBinder method. And now, we
need one more XML file. Create syncadapter.xml with
root element sync-adapter. Once again, this XML
file defines the settings associated with our sync-adapter.
Including it's content authority. The account type that it
syncs. Whether or not it's user visible. Whether it
supports uploading, which changes the way the content provider
interacts with the sync adapter. Allowing parallel syncs and is
always syncable. These settings make sense for this particular
application, and I bet you know what comes next. You're
right. You have to register the sync adapter service
with the package manager. And therefore we have to create
more manifest entries, containing some important metadata. Most
importantly, links to the file we just created. All
right, now we're getting close. Let's start working
on the sync adapter, itself. We'll start with a
helper function to get the dummy sync account
and make sure that it has been created. Then,
we'll add another helper function to our sync adaptor,
to make it easier to test our sync adaptor.


20 - Finish the SyncAdapter
===========================

All right, here's a big one. Finish
the SynchAdapter, making it fetch the weather
and store it in the database. Alter
the updateWeather function within the ForecastFragment to
start a sync with the SyncAdapter. Some things here. Pull the code from on
handle intent into our SyncAdapter. On handle
intent is inside of our Sunshine service.
The good news is that abstract threaded
sync adapter provides a background thread to
run the server fetch on, just like
intent service does. Also, just fetch the location
query from our utility class. Eventually, we
want to run syncs like this without any
involvement of the user. Finally, make the
sync adaptor run when we call Update Weather.


21 - Finish the SyncAdapter
===========================

All right, you're done. Lets look at the
solution. We'll begin by adding a log tag
into our abstract threaded SyncAdapter. Since this stuff
runs in the background, it's really helpful to have
some logging. Since we ultimately want to run
the SyncAdapter in undetected mode. Let's pull the location
query from our utility class. And then we
paste in the code from our existing sunshine service.
We'll have to patch a few things up. We'll have to call getContext to
get the current context, for example. Note that I copied over ad location at
the same time I copied over the primary function. And then finally, we'll fix
update weather to use the new helper
function in our SyncAdapter. Let's run this.


22 - Scheduled Synchronization
==============================

Well, we're now using the sync adapter, and things are
working pretty much as before. We want the app to do
this synchronization cleverly, and we'd like to get rid of
that old Refresh menu item. Let's start by cleaning up all
the other routines we have to sync. So we certainly
don't need any of this other stuff like FetchWeatherTask, or all
this stuff we did in Sunshine service, and we'll want to clean
up the manifest accordingly. We certainly don't need either one of
these anymore. And in Preferences, we can just change
that to sync immediately. So now we're really using this
sync adapter everywhere. We have a problem though. We're not
being very smart. The user still has all sorts of
places where they see an empty list. We want to
sync more intelligently. In Android 2.2 Froyo, Android added the
ability to have sync adapter's sync periodically. We can add
a helper method to do this in our sync adapter.
The problem is, this method isn't as smart as
we'd like it to be, it still won't do all
that cool batching with exact repeating alarms that we'd
like it to. Fortunately, we've added something that does just
that, but it's not available until API level 19.
Taking advantage of flexible time to do inexact repeating arms,
let's set some nice defaults for our Sunshine sync adapter.
First we'll add these constants. To make things a little
clearer in our code let's add another function
that we'll call when a new account is created,
and here we'll set some important flags. Such as
configurePeriodicSync, the one we
just created. SetSyncAutomatically, without which
our periodic sync will not be enabled. Since we're
just starting off let's do an immediate sync, then
we can call it from a strategic place and
get sync account. Finally, we can make the interface
to the world a little bit cleaner by
adding an initializeSyncAdapter function. That's simply makes sure
that an account has been created. And now
inside of the main activity in the onCreate, we
can just make a call to that new
function. And it'll make sure that the parameters for
our sync adapter are set up correctly. Lets
see if this has any impact on our emulator.
The new version of Sunshine right from the start shows all of current
weather. For a sync with a sync
adapter to happen successfully at periodic intervals
in the background you must: have a
ContentProvider marked as syncable, enable automatic sync
for the SyncAdapter, do an initial immediate
sync, or set an interval in milliseconds.


23 - Scheduled Synchronization
==============================

You indeed must have a content provider marked as syncable, as well as
enable automatic sync for the sync adapter.
You don't need to schedule an immediate
sync, although it's nice for users, as we did. And, this is a minor
note, you don't set the interval in milliseconds, but instead set it in seconds.


24 - Google cloud messaging
===========================

Inexact repeating alarms. Infinitely better than exact repeating alarms, but
still far from ideal. The problem with any kind of repeating
alarm is that it's still polling your server to check
for updates. So the more frequently you poll, the fresher the
data you can display, but the higher the cost in
battery life. You can pull as frequently to conserve battery but
that just means your content will be stale for longer.
You could just let the user decide the update frequency themselves,
but then you lose the magic. If only there was
a better way. Is such a thing possible? Yes it
is. Google cloud messaging lets your server notify your app
directly when there's data ready to be downloaded. Or it
can even include the new data in the message payload
itself. Using Google Cloud Messaging, you can send messages from
your server to any installed instance of your app via
the Google Cloud. As a result, you can stop polling,
which will immediately improve battery life and also improve the
freshness of your app. And instead, rely on your server
notifying clients when there's data to sync. These messages can
be simple tickles, that trigger a sync adapter by notifying your
app that there is new data or need to download.
Or you can include the new data within the message payload.
In the sunshine example, we're using someone else's server. But
even then, it may make sense to create your own middle
tier that pulls the source and notifies your installed app instances
when it notices a change. Now we're not going to set up a
server in this lesson, but you can see the full details for
using Google cloud messaging from the developer guide linked to instructor nets.


25 - Notifications
==================

One of the most powerful features of Android, since the
start of the platform has been the ability to deliver
timely notifications to users. We're going to add a simple
one to our weather app. This simple notification will show the
weather icon for the forecast, the forecast text, and the
high and low temperature for the day. One of the key
things we wanted to point out is how not to
be spammy with our notification. Our app will display at most
a single notification. It really wouldn't make sense if these
notifications stacked up anyhow. They don't display enough information to
give context. The key thing with notifications is to deliver
a useful morsel of information, formatted in a standard way, so
that it harmonizes with the rest of the system. We'll
start by adding a string for the preference for the
last time we sent a notification to the user. As
well as a format string for the body of the notification.
Let's implement the notification inside of our sync adapter.
Our notification will be based upon what is in
the database. So, we'll create projection and column indices
value in our sync adapter, for the weather id. Description,
max and then temperature, let's add a function to
notify us of the weather. We'll add some additional
constants up here. One for day in mili's and
one so that we an use the same notification ID.
Reusing the notification ID means that our application will only
post at most one notification. And then finally, we'll call
this function at a reasonable place within our on perform
sync function within our sync adapter. Inside of notify weather, we'll
check to see whether or not, we've actually shown notification
that day. If we haven't, then we'll talk to the database.
Get a cursor for the current resolver for the current
day, and then fetch the data, but one thing were not
doing is actually showing the notification. Now all right, were getting near
the end. Implement a weather notification,
build our notification using Notification Compat.builder
pointing to a Pending Intent built by the v4 compatible. TaskStackBuilder and
sent with the NotificationManager. Hint: an
explicit intent to our main activity is
a good idea here. There's lots of documentation on this you'll want to read.


26 - Notifications
==================

All right, you're done. There were a lot of new things that time. But, afterall,
you're becoming a seasoned Android programmer by now.
As I mentioned before, we're going to use NotificationCompat.Builder
to build our notification. It's easy to build
a nice looking notification that has our icon
representing the weather forecast, the title for our
App. And our content text, the forecast with
highs and lows. We're going to use another class
from the support library, to create a task stack builder
for our pending intent. This is a simple case
of task stack builder, because all we have is a
single item on our stack. We just add the
next intent and use it to build out pending intent
that we pass into the notification manager. Finally we call
the notification manager with the built intent from our builder,
with a notify function. One of the great things about notification manager is
it can be used from any thread even though it is displaying UI.


27 - The Power of Notifications
===============================

If an app updates in the background, but no one
opens it to find out, did it really happen? Notification
started off as a convenient way to notify users of
background updates. But they've grown to become a powerful standardized shortcut
to interact directly with apps in a light weight way.
Starting in Jelly Bean it was possible to expand notifications, making
them larger and containing more information. That also introduced the
concept of the rich notification, which is able to include actions
like these, to be performed on the data
included within the notification. This speeds up interaction and
helps users streamline their notification triage experience. As a
standardized UI designed specifically for conveying timely information in
limited screen space. Notifications were the obvious mechanism
to use, when Android expanded in to wearables with
Android Wear. Suddenly those rich notifications that you designed
for phones and tablets make your app wearable compatible,
for free. Don't overdo it though, there's a fine line between
useful and spammy. And it's only a couple of clicks for a
user to disable your app's notifications
permanently. So check out the instruction
that's below for links to the design guide in creating good notifications.


28 - Turning Weather Notifications OnOff
========================================

All right. We're just going to add a few more details to the app.
We're going to implement a weather notification
preference. We'll do this by adding a
new preference to the app settings to turn off notifications. And then, use
that preference to turn off the notification.
Hint, you're going to want to use a CheckBoxPreference


29 - Turning Weather Notifications OnOff
========================================

Alright, you're done. Let's start by adding all of
the strings we're going to need for our new preference.
Key, label, and something like true, false, and default.
Then we'll additional preference into pref general xnl, a check
box preference that uses those strings we just defined.
Let's go over to our sync adaptor. In the notify
function, we add the code to fetch the preference
and make use of it. And there we have it.
Now we won't display notifications if the user doesn't want us
to, which is a great thing for an app to do.


30 - Delete Old Weather Data
============================

One of the things you may have noticed, is that our app database continues
to grow forever, eventually filling up the device. This is a great way to
get out app uninstalled. So let's fix this. Add code to delete weather data that
is more than one day old. Use the Calendar function to do data arithmetic.


31 - Delete Old Weather Data - Solution
=======================================

All right, you're done. Let's look at the solution. If you
guessed that we were going add this code to Sunshine sync adapter,
you would've been right. We're actually going to place the code right
here, because this is where we know we've done a successful insert.
So what do we do? So we start off with the
calendar object, for the current day. Then we add negative 1 to
the date which means it points to yesterday. Finally, we convert
that into a database-friendly string. And then we do a query to
delete everything less than or equal to that day.
Done. It's always good to clean up after yourself.


32 - Update Map Intent
======================

All of you detail-oriented people probably noticed that
we never actually used the coordinates we get
from the server for location, relying instead on
both the maps API, and the weather API to
do the same thing with location queries. Unfortunately,
they don't always agree. Implement maps using the coordinates
stored in the location table. You'll want to start
by moving the menu code from MainActivity to WeatherFragment.


33 - Update Map Intent - Solution
=================================

All right. And here's the solution. To start off, let's
look at our forecast fragment query. Since it's a join
between two tables, it's really easy for us to add
additional parameters to our query. Now we make sure we keep
the column indices consistent. Now we've grabbed the latitude and
longitude at the same time we're grabbing our weather entries.
The next thing to do is to move the function
open preferred location in map over to forecast fragment. Instead of
using shared preferences to get these values, we can actually
get the cursor from the forecast adapter. We can get
our cursor moved to a reasonable position, and then build
our new string which is just latitude colon longitude. We'll
leave our comment where it was. Finally, we just have
to fix up a few things in the code and
that's it. Well, at least for the code. We still
need to do some work in XML. Let's move this R.ID.action
map now over to forecast fragment inside of it on
options item selected. And now we've got to do some
XML work. Let's look at our main menu. We'll just
pull this action map item out, and we'll place it right
into forecast fragment. All right. One more thing to do
while we're here. One of the things Raito talked about was
wanting to get rid of the refresh menu item. So,
let's just comment it out. When we remove that action refresh
item, we should probably comment it out from our source
code too. After all, you never know when you'll want to
use that for debugging. So, there we have it. Our
final app. No longer having a refresh button. With a
new exciting setting to turn off and on weather notifications.
Details are taken care of. I hope you've enjoyed building
Sunshine. There's still lots of things left to do in
Sunshine. For example, we could also add a lot more intelligent
things to its user interface. And of course, we
could make it a lot smarter about synching data.
And I hope the practices you've learned building Sunshine
will help you in all of your future applications


34 - Lesson 6 Recap
===================

Congratulations. You now know how to build an
app that runs beautifully in the foreground, and efficiently
in the background. In this lesson, you learned how
to use services and think adapters, as well as
creating rich notifications, all ways to provide a
great user experience, even when your app isn't visible.
Join me now for the final story time, as
I take you into a possible future for Android.


35 - Storytime Future of Android
================================

I was 12 years old when I got my first modem, a 1200 baud ACE compatible. I
started getting online, not the internet online, local
BBSs. At around the same time, yuppie was supporting
the first brick-sized mobile phones as the first
GSM network started rolling out. Fast forward less than
less than a year by Android. And almost overnight,
we've come to expect the world's information in the
palm of our hands. Just six years after the first
Android phone launched, it's powering everything from phones to
tablets, watches, TVs and cars. So where do we go
next? The most obvious answer is enabling the same
handheld cloud connecting computing platform to the next 5 billion
people. As hardware becomes cheaper and more powerful, more of
the world's population will be able to get internet-connected computers.
And the smartphone is going to be what makes
it possible. To facilitate this, we're seeing new approaches
to getting online, mesh networks, white spaces, or even
Google's Project Loon. We'll hopefully soon see ubiquitous world-wide internet
connectivity become a reality. At the same time, smartphones
will start to disappear. In the same way that
we don't think of computers as computers once they
become part of our dishwashers, cars, or TVs, we'll stop
thinking of mobiles as such when they disappear into our
watches and [UNKNOWN]. We'll come to expect that anything that
can be improved through the incorporation of a wireless internet
connection will simply have one. For that to happen, we need
to find more creative ways to interact with computers than
using mouse, keyboards and touch screens. I can all ready pick
up my phone or lift my wrist, address Google, and
perform searches, send texts or email, set alarms, or take notes
without having to touch a screen or keyboard. Screen
themselves have become so sharp that pixel densities aren't
perceptible to the human eye. And the team at
Google X are building contact lenses capable of measuring blood
sugar levels. Speaking of senses, not only can I
control my phone by putting it either face up or
face down, it has a battery of sensors similar
to an [UNKNOWN] class warship. Light meters, temperature changes, barometers,
gyroscopes, all of which can be used to
replace text entry or automate processes. Wearables, cars,
and thermostats introduce even more possibilities of sensors.
Connect all of that to the cloud, and the
possibilities are endless. Future of Android isn't a
more powerful phone operating system. It is the
brains behind invisible, ubiquitous, cloud connected computing. So
the skills we learn today to build cool apps
are the same skills we'll use tomorrow
to control everything around us. We've taken
the first step into that future. I can't wait to see what you do next.


36 - Congratulations Android Party
==================================

[SOUND].
>> [LAUGH]
>> Content providers.
>> Congratulations, everyone, you've finished building the Sunshine App.
>> But there's always room for more sunshine.
>> There is, Dan, but now it's time
for the students to go build their final project.
>> And you can get a lot of the resources you
need to build really great mobile apps at the Android developer site.
>> And stay tuned for more from us and Udacity.
>> For now, I think it's
time to celebrate with some cake.
>> Woohoo.
>> Lemon tarts.
[MUSIC]
