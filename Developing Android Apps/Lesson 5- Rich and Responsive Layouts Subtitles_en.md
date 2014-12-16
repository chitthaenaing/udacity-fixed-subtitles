
01 - Intro to Lesson 5
======================

Now that we're storing the data we downloaded in
a sensible fashion, we're ready to build rich UIs
for each of our activities. That means learning about
fragments and how we can use them to create
complex user interfaces that can easily be adapted to
work seamlessly on a range of different devices and
screens. But first I'd like to tell you a
little more about the principles behind great Android design.





02 - Android Design Principles
==============================

Enchant me. Simplify my life. Make me amazing. They're
incredibly one-sided wedding vows, and the three key principles
underlying the Android team's creative vision for the platform
and the apps that run on it. As engineers,
it can sometimes be difficult to see past our
own delight at harnessing the power of an sdk
to bend a machine to our will, but ultimately,
our goal must be to create that same empowering experience
for our users. And we do that by creating apps
that are as esthetically pleasing as they are functional and
easy to use. Users judge the quality of your app
within the first 30 seconds. It may not be fair,
but a disproportionate amount of that judgement will be based
not on the functionality, but on the visual aesthetics. Does
it look polished? Does it look professional? How easy is
it to use? More than just looking pretty, the entire user
experience is critically important. With 30 seconds to
win users over, it's critical that that onboarding process
the time and effort required to go from downloading
your app to performing it's main function should be
as short and frictionless as possible. Not only
that, but a well designed app should evoke a
visceral reaction that speaks to the subconscious. It should
be fun to use. It should surprise in delightful
ways through subtle animations and smooth transitions that contribute
to a feeling of power and effortlessness. It should
let users touch and interact with objects directly rather
than having to use buttons and menus. And it should
use rich imagery and pictures in place of lots
of words and long sentences. It allows users to
customize your app, to make it theirs while providing
beautiful and sensible defaults. Create something that works like magic,
learning your preference in the context provided by
the device so it never asks users for information
that they've already provided. Provide simple shortcuts to complete
complex tasks, and remember data settings and customizations making
them available across every device. While it's good practice
to create a familiar and welcoming experience by creating
a look and feel that's consistent with the platforms
styles and themes, it's just as important to remember
that this, and all the other principles I've discussed
here, are really just a starting point for your
own creative vision. Deviation from the guidelines is encouraged,
but when you do do so, deviate with purpose.





03 - Enjoyable Apps
===================

Before we move on, take a moment to consider some of your favorite apps.
What do they have in common? What about them makes them enjoyable to use? Select
your favorite examples and post screenshots in
the forum. Describe how the design of
the app contributes to your enjoyment using
it and click this box when you're done.





04 - Enjoyable Apps
===================

Thank you for posting your favorite apps on the forum, we'll be checking
in on them periodically. But for now, let's get back to building Sunshine.





05 - Recap on Views and ViewGroups
==================================

Great. So to move one step closer to building awesome apps like the ones you
submitted to the discussion forum, we're going to learn about how to build up
the Sunshine UI. We'll do a quick recap first to help remind you of the concepts
that you learned in lesson one. In order to build a UI in Android, we use views.
Views are rectangles on the screen, and we may or may not see the borders of
that rectangle. Essentially, a view handles drawing and event handling. And
all the basic widgets in Android extend from this base class. The Android design
guide has visual examples of these basic building blocks of apps. For example,
we have the basic text field, as well as editable text fields.
We can do auto complete on them, and it comes with text selection as well.
Where it pops up a contextual action bar, for you to copy and paste.
There are also buttons, and also a suggestion to use borderless buttons.
It also has spinners, where you can select from a drop down menu.
There's also checkboxes, radio buttons, on/off switches, and
they also have some progress bars. There's a ton more basic widgets here, and
also in the Java docs. Check those out. And even ones that aren't listed here,
so. You'll come across them as you need them.
Now, if you want to display multiple views together, you'll need a view group.
A view group is a container for children views. Here are a couple of
common view groups, which Dan introduced earlier. First, we have a frame layout.
A child that gets added will be default positioned in the top left corner of
the view group. If you add a second view here, it will overlap the first one. So
typically we only have one child per frame layout. Next we have a linear layout,
it's composed of children either in a horizontal row or in a vertical column.
We can also specify layout weight. Lay out weight allows us to
distribute the width or height of a parent across the children. For example,
this has weight one, and this has weight one then the width of the parent can be
split across them evenly. Then we have a relative layout where we can specify
that a view should be aligned to the parent's left, top, right or bottom edge.
We can also specify that one view should be relative to another view.
It isn't full if the space is nicely compared to a linear layout, but
there are pros and cons to each. There's a grid layout where the views fill
up cells in a grid. You can also have views that span multiple cells.
I also want to point out that a view group is a view.
So in our code when we refer to a list item layout as being a single view.
That just means the root view of the whole view hierarchy for that layout.
Chances are that root view is a ViewGroup, so it contains children views as well
as ViewGroups. So basically you can nest ViewGroups within ViewGroups.
The reason why we care so much about parent child view relationships,
is because the way a child view gets laid out depends on its parent.
The simplest example of this is specifying a view's width and
height. By now you've seen that every view requires a height and
width. The two possible values are either wrap content or match parent.
This diagram shows all the possible combinations for width and height for
this text view. This is the balance of the view when we set wrap_content for
height and width. This is what happens when we do match_parent for
the width and match_parent for the height. And this is what happens when
both are set as match_parent. It shows the full dimensions of the parent.
When you run this on the device. All of these would visually look the same,
because you wouldn't see these boundaries. So why does it matter which one
we pick? Well, it matters as soon as you have other children that need to
be beside this view. And another reason is you want to specify gravity.
Let me spend a quick moment explaining gravity. Say for example, you
have this text view within a frame layout. By default, the text is left aligned.
And in this case, no gravity is set. If we specify gravity equals center,
then it will center the content within the text view. Vertically it can't move,
'because it's already centered, but horizontally it does shift over to be in
the middle. Instead of this. If we specify layout gravity equals center,
that means to center horizontally and vertically within the parent.
So we grab the whole child text view and we move it into the center.
Now it can't be centered horizontally, because it already takes up the max width
of the parent. If we really want to move the content to the middle,
then we should specify wrap content on this text view, so that it can be
moved to the center with this attribute. You can also specify padding and
margin on views. For example, if you added padding on this text view,
then it would shift the content inside by x amount on all sides.
If you specify layout margin to be x, then remember that the parent is
the one who will be interpreting this layout ground. In this case,
it adds a margin of x all around the text view. The text view is only this size.
When you render both of these, they would visually look the same.
Here's one case where it could matter. If this was a button and
you pressed it, there could be a gray background here showing. In some cases you
may want padding or margin or both. All views have a visibility property. For
this image view, it can be visible, invisible, or gone.
If it's invisible then it's not shown, but there's still a place holder for
it, we still have to go and measure the size of it. If it's gone,
then it's not even in the layout. It's as if it didn't exist in the XML.
You can also toggle the visibility of a view dynamically during runtime.
In the java doc, you can find all the possible XML attributes for that class.
It also shows inherited XML attributes, for example, from the view class.
For this image view, and then it shows the corresponding Java methods for
those attributes. Now as a briefer for [UNKNOWN] basics to help us get started,
but definitely check out the developer site for
more details later. Much like the real world, if you were working with a team to
build an Android app, you would start by implementing the wire frames first.
This has a correct flow for how the user will interact with the app. But
it doesn't have the final visual look and feel yet. We'll build this up step by
step, starting with the main activity then we'll move on to the detail activity
and then later build up the tablet UI. Then we'll receive a set of visual marks
red lines and assets, all of this will help us build a pixel perfect layout





06 - Building List Item
=======================

To build out our UI, let's start with the wire
frame for the main activity. It displays a list of
forecasts, where each item contains an icon, a date, weather
description; as well as high and low temperatures for that
day. Since today's weather is probably most relevant to the
user, we give more prominence to it as a list
item compared to the other days. We'll start off by
building the list item layout for future days because it's simpler.
And then afterwards, we'll come back to do the today layout.
In the current implementation, the list item forecast layout XML is
a horizontal linear layout of four text views. We'll need to
modify the layout so that it looks like this wire frame.
We should divide and conquer until we can use the view
group layouts that we're familiar with. Can you imagine how you'd
break down this UI into components that you know how to
build? As a hint, what if I draw these separators here?
Within those elements, can you find a way to
break it down even further with view groups you're familiar
with? You'll need nested view groups, which means view groups
with children view groups. As a tip, this data will
be populated dynamically at run time. But you can
test it by hard coding some text values such as
Tomorrow for the date or Clear for the weather condition.
For images, we can specify a placeholder drawable called ic_launcher.
It's named this way, because it's the launcher icon, which is also
known as the app icon. And this is already included in our
app. We'll be getting the rest of our weather icons for our
app later in this lesson. If you want to see what this layout
looks like, you can click on the design pane to see a
preview of it. On the right, we see a component tree listing
out the different views. We can also see for a given view
what the properties are, and their values. We can change the orientation
of the device that your previewing, you can also
select different devices such as Nexus 7, or Nexus
same time. As you build out your layout for
this task, don't worry about the font color, or
font size, or any other visual details until later.
After you compile and run, the app should look
something like this. Check this box when you're done.





07 - Building List Item Solution
================================

To build up this list item we created a horizontal linear layout with three
children. The first child is an image view. The second child is a vertical
linear layout composed of two text views stacked on top of each other.
The third child is the same. It's a vertical linear layout composed of
the two temperature text views stacked on top of each other.
Now you might be wondering why we didn't use a relative layout.
Because that would give us less layers of hierarchy. Well, relative layouts
are good if you want to specify that one view should be to the right of another,
or on top of another view. Relative layouts are also good if you want a view to
be aligned to the parents left, top, right or bottom. But it's harder in this
case to center these two text views within the vertical height allocated by this
image view. It's easier with the linear layout here and here, so if the image
was any taller, these two text views would still stay vertically centered. And
this is what it looks like in the Device Preview within Android Studio.
I want to point out one thing about this horizontal linear layout,
which has three children. The image view, the vertical linear layout, and
the other vertical linear layout. The second child actually has a width of zero
DP but a weight of one. That means that any horizontal space that's not taken up
by other children will be distributed among views that have assigned weights. So
if we look at this third child, it has a width of wrap content and
a weight of zero. If we gave it a weight of one,
then it would stretch out like this. Basically the icon is a fixed width, and
then the remaining horizontal space is divided among these two children.
Since they both have a weight one, it's distributed equally. But if you look at
the wire frames, these temperature views are actually aligned to the right. So
all we need is for the width to be wrap content, and for the weight to be zero.
Then any remaining horizontal space will be assigned to this middle element,
with a weight of one. And this is the code for the xml layout.
We have a linear layout in horizontal orientation. The width is match parent to
match the width of the screen, and each list item has wrap content on its
height. But it does have a minimum height at least of this attribute,
list preferred item height. We specify gravity to be center vertical so that
all of its children are vertically centered within it. We also specify a little
bit of padding. Inside of it, we add an image view, which has wrap content for
height and width. And we specify the source as this placeholder drawable.
Next we have the vertical linear layout, where the width is zero DP and
the weight is one. We specify a little bit of padding between this view and
the icon beside it. Within the linear layout,
we have the date_textview as well as the forecast_textview. Beside that,
we have another vertical linear layout with the high temperature_textview and
the low temperature_textview. And that's it.





08 - Building Todays List Item
==============================

Next, we'll create the Today List Item. Since this layer is different from
the other list items, we're going to need to create a new XML file under
the resources layout folder. Use the same place holder image as you did before.
Also use the same IDs that you declared in the list_ item_forecas_xml file.
For example, in our code we called this list item icon, and
we callled this list item date text view. So we use those same ID's for
the elements in this XML file. You'll see why when we introduce view holders
later. After you've finished adding it, make sure the app still compiles.
You won't see any visual difference until the next step.





09 - Building Todays List Item
==============================

Let me the explain the reasoning that I use when I
look at a new layout. It looks like there's two equally spaced
columns here. That signals to me we need linear layout weights,
where each of these have equal weights. So let's create a horizontal
linear layout. Within this first chart, we have three vertically stacked
text views, so we have a vertical linear layout, and the same
goes for this second chart with has two elements vertically stacked
on top of each other, which means we use a vertical linear
layout as well. And here's the code, we created a
new list item forecasts today XML file under the layout folder.
The code looks very similar to list item forecast. I
has a horizontal linear layout, as a root element. The first
channel is a vertical linear layout with the width zero
and a weight of one. The second child is another vertical
linear layout with a width of zero and a weight
of one, that way the horizontal space will be distributed evenly
among each child. If you go back to the first
linear layout, notice that we have three text views within
it. For date, high temperature, and low temperature. We specify
gravity to be center horizontal. So anything within the linear
layout will be centered horizontally. Otherwise, by default, they would
be left aligned to the linear layout. The same applies
for the second linear layout. We specify gravity to be
center horizontal so that the icon and the weather forecast
text view within it are also centered horizontally.





10 - CursorAdapter
==================

We created the today layout so why doesn't it show
up? The reason is because we're using a simple CursorAdapter. Remember
that it creates a list item for every row in the
cursor but it only supports one item view type. That means
that all list items must use the same layout. If
we want today's forecast to look differently we're going to need
to create a custom CursorAdapter. That way we can have multiple
item view types. Then we can have different list item layouts
based on what row of the cursor we're looking at.
Note that the concept of item view types applies to
adapters in general, not just the CursorAdapter. If you open
up the documentation for CursorAdapter, you'll see that it's an abstract
class. It contains two abstract methods that you'll need to
override. The bindView method as well as the newView method.
The newView method knows how to return a new list
item layout, but doesn't contain data yet. The bindView method knows
how to take an existing layout and update
it with the data pointed to by the cursor





11 - Create ForecastAdapter
===========================

Open up the Sunshine app. And we'll create a
new file for our ForecastAdapter, which extends cursor adapter. We
provided you with a gist of code to get started
below. The code overrides the newView method, where it inflates
the list item forecast layout XML. It also provides
an implementation for bindView, where it reads from the cursor,
and updates the layout. For example, from the cursor, we
read the weather forecast description. Then we go and find
the text view for that within the view hierarchy of the
list item layout. We look for the text view with the id
list_item_forecast_textview. Then we update the
textview with the right description. We
also left some to dos in the code. So, go ahead and
copy over the file and then finish the to dos. In
the current implementation of the app, all dates are displayed the same.
In the wireframes though, you'll notice that we have a more friendly
date format. In the gist we also included some helper methods and
strings for date formatting. That's because in our current
implementation, all the dates are displayed in the same way.
In the wireframes though, we have a more friendly date
format. Such as today, tomorrow, Wednesday, Thursday, etc. Here's that
logic for what the helper method does to get the
friendly date string. If it's this week it says today,
or tomorrow, or the day of the week. If it's
more than a week out then we use the format
Monday, June 8th and so on. Once you finish the
to dos and you added the helper methods and strings,
then go ahead and replace the simple cursor adapter with
a forecast adapter. After you compile and run the app it'll
look the same as before with only one list item
view type shown, except now it's using a ForecastAdapter and
now it has better date descriptions. I know that I
promised you that cursor adapters could handle multiple item view types,
so we'll do that in the next step.





12 - Create ForecastAdapter
===========================

Here's the solution. In the utility class, we add the
helper methods that were provide in the gist. We also
declared these strings in the strings.xml file. In the Forecast
Adapter Class, in the bind view method, we finish off the
remaining to do's. After we read the high temperature value
from the cursor, we go and try to find the
text view, represented by the ID, list item high text
view. Then we use a utility function to format the temperature
so that it can be displayed in the text view. Then we do the
same for the low temperature value. In the forecast fragment class, in the on
create view method, we remove the use of the simple cursor adapter. Instead, we
use the new forecast adapter and then we set it on the list view.





13 - Two Item View Types
========================

Once a forecast adapter works, we want to modify it so
that it can return a second item view type for the
today list item. Normally in the sub class, getViewTypeCount return to
one, but we're going to override it so it returns two for
the two different layouts. But how does it know when to
use one there or the other? Well, we override getItemViewType so
that when the position in the list is zero, then we
say that it's the TODAY view type. Otherwise, it's the FUTURE_DAY
view type. These two view types are declared up
above. These are just integer representations of the view type.
The numbering has to start at zero because these values
can not be greater than or equal to getViewTypeCount. So,
the possible values for us are zero and one,
so that we know that zero always maps to the
view_type_today and that one always maps to view_type_future_day. We're going to
use this information in the new View method. Remember that
previously we inflated the list_item_forecast.xml. Now, we're going to use
the getItemViewType to determine whether we should use one
layout or the other. Fill in the to do
to address this behavior. LayoutId refers to resource ID,
which is in the form of r.layout.something. We don't
have to reform the bindView method because it will
continue to work. That's because the IDs are the
same between the today layout and the future day layout.
This is what your app should look like when you're
done. The today layout is different from the rest of the
days. Go ahead and make these changes in your app and
then finish the To Do. Check this box when you're done.





14 - Two Item View Types
========================

To finish [INAUDIBLE] to do we modify the
new view method to choose layout ID based on
view type. If the view type is today then
we use this resource ID. This points to the
layout xml file list item forecast today. If the
list type is for a future day, then we
use this resource ID. This points to the list
item forecast layout xml. Then we inflate that layout.





15 - Using the ViewHolder Pattern
=================================

In the adaptor bindView method, we have to traverse the view
hierarchy to find all these different views to set data onto
them. If it's a recycled view, meaning that it was used
in the list previously to display other data, we still have
to traverse and find all the views. To remove unnecessary find
View By ID calls, we can use the View Holder pattern.
For a list item layout that contains different views, we can
create a View Holder object. It contains member variables that reference each
view in the layout. The View Holder object is stored in
the tag field of the view. The next time that the view
is recycled and used again. We can just immediately set the data
onto these fields. You don't have to go find all the views
again. This is our View Holder class. You can name it anything
you want, it's just a Java class. Given the list item layout,
we do all the find view by ID calls. That way, we
can hold references to all the child views. In the newView method,
after we've inflated the view, we create a new View Holder object
from that view. Then we set the View Holder as the tag of
the view. The tag can be used to store any object. But
don't abuse it, because when you read it back, you have to know
what you stored in there. A View Holder is a good use
case for it. Then in the bindView method. We read from the tag,
to get back the View Holder object. And we immediately have references, to
all the individual views we need to update, such as the Icon View,
the Date View, the Description View etc. Go ahead and define a View
Holder class and update your adapter to use it. When you compile and
run, you may not see that noticeable of a difference. But now your
list will perform better when it scales to hundreds or thousands of items.





16 - Formatting Strings
=======================

Currently, this is our forecast list. It looks okay, but
it would be nice if we could show the degree
symbol for these temperature values. Using this notation is best
practice. It will help the translator know how to reorder the
text and the parameters so that it fits the local
language. Now let's look at how we can use this
method to format temperatures within our app. Within the string.xml
file, we declared the format temperature string resource. We use xliff
tag to denote that this is where the decimal temperature
value will go. This is followed by the unit code
character for the degrees symbol. In the utility class, in
the format temperature method, we can use this string resource. Remember
that, in this method, we take in a temperature and
a user's preference for metric or imperial. Then, we return
the converted temperature. We're going to modify this method so that
it also returns the formatted string with a degrees symbol after
the value. In order to do this. We need to past
in a context. From the context we can get access to the
string resource ID that we declared earlier. Then we passed in any
additional parameters for that string template. In this case we only have
one parameters so we pass in the temperature value. Then this
fully formatted string gets returned to the caller. This method is used
in the forecast list, as well in the detail page. So we
also had to update the code there so that it would compile
with this new parameter. And this is what the app looks like after
the change. Go ahead and make the change in your code to add a
string resource for displaying temperature in
degrees. Check this box when you're done.
And remember, use this notation going forward
for when you need to format strings





17 - Coding the Details Screen
==============================

You've implemented the main activity wireframe. So, now, we can move
on to the detail activity wireframe. This is what it looks
like. We have the date, the high and low temperatures, and
then additional weather information. We also have the weather icon and
the weather forecast. And these line up horizontally. Build up the
layout XML for this wireframe. Then modify the detail fragments that
you can populate all this information, including these new views. While
you're doing that, you might as well move the detail fragment
into its own file, so it's separate from the detail activity. At
this point the detail fragment class is getting pretty long so you can
move that into it its own file. Before you get started, I want
to point out that this layout has information taking up a lot of
vertical space. If you rotate your device into landscape mode, or you have
a smaller height device, some of the content could get cut off. So
think about how you can make this layout be vertically scrollable. To show
you what I mean, I added a couple more lines of text at
the bottom of the layout. Then I made the content vertically scrollable
so you can see all the information. So go ahead and make that
change to your layout as well. When you're done with all these
steps, this is what the app should look like. We don't care about
the visual details now. We just want to make sure the functionality is
hooked up, so all the right information is displayed on screen. You can
use the code snippets provided below
for helper functions and strings that you're
going to need. And again, leaving a placeholder image is fine for now





18 - Coding the Details Screen
==============================

So how did you break up the UI into smaller components?
Well, you might have thought that this could be two columns, but
then it would be hard to position the elements here. You
could center them vertically within the height of the screen, but it
wouldn't necessarily line up with the temperature views on the left-hand
side. Instead, this looks more like a horizontal linear layout. Then, the
rest of the elements could be laid out by using a vertical
linear layout. The vertical linear layout would have six children. The third
element would be a horizontal linear layout, composed of two children.
The first would be another vertical linear layout with these two texts
views, and the second element would be another vertical linear layout
composed of these two 2 elements. To make the contents of the
screen vertically scrollable, we put it inside a scroll view. Using
a list view here would be overkill, because we don't need to
scale to an infinite number of item and we don't need recycling.
There's a fixed number of fields on the screen, so a scroll
view is the perfect choice. I can show you our implementation for
the fragment detail XML layout. We hard coded some data in the
layout, so that it would show up as a preview in the
Design Pane. At the root of the View hierarchy, we have a Scroll
View. Scroll Views can only have max one child. So we set
that to be the vertical linear layout. Inside of this layout, we have
a text view for the day of the week, the calendar date,
and then a horizontal linear layout. This is followed by the humidity text
view, wind text view and pressure text view. In the XML
code, we see the scroll view with the trial linear layout. We
give it some padding of 16dp, so that the content does
not flush up against the edge of the screen. Then we see
the text views followed by the horizontal linear layout. We specify
layout margin top of 16dp to give it some more space from
the bottom of this text view. Within this horizontal linear layout, we
have one vertical linear layout which has a width of zero and
a weight of one. And another linear layout with the
width at zero and a weight of one. That means that
both of these children have equal width. For this linear layout,
we specified gravity to be center horizontal. That means that the
contents inside of it will be centered horizontally, which includes
the icon, as well as the forecast text view. Lastly, we
have the remaining text views for the other weather details. When
the layout looks good, we update the detail fragment. At this
point, we also move it into its own file. In the
onload finish method, we used to have a lot of Find You
by ID calls. However, we can cash those views earlier so that
we only have to fetch them once. In the onCreateView method, once
the view is inflated, we can go and find a reference
to all the views we'll need later. We store these views as
member variables in a class. Which is why they start with the
letter M. In the onCreateLoaded method, we make sure that the projection
that we're requesting from the content provider contains all the information
that we need. Especially the new fields that we just added views
for. Then in the onload Finish method, we get the cursor
back with the data we need. We read the weather condition ID
from the cursor. And we're going to need this later to determine
the right icon. But for now, we can just use a placeholder
icon. We continue reading from the cursor and updating the views such
as for the date and description and temperatures. For the new views
for humidity, wind, and pressure, we read the
information from the cursor, and then we format it
properly for the UI. This involved copying over
the strings and the utility method from the gist.





19 - Optimizing Layouts
=======================

Now that you know how to create an
arbitrarily complex nested layout, it's important to understand
that they don't come for free. Once again,
the resource constraints of the platform spoil the party.
[SOUND] Inflating complex layouts can be expensive, potentially
impacting the performance and responsiveness of your app.
So there are two good rules of thumb
to help. First, keep your layout shallow and wide,
rather than deep and narrow. That means you're better
off with more siblings and fewer children. Because it's
never as simple as that, you also want to
avoid having excessive views. In the most general terms, that
means your activities' full hierarchy should never have more
than ten nested views or over 80 views in
total. That probably sounds like a lot, but let's
open up the Hierarchy Viewer tool in Android studio and
see just how quickly that can add up





20 - Hierarchy Viewer
=====================

Here you can see hierarchy view lets you select from
a list of devices with physical and virtual on the left,
each of which then contains each of the running activities
and applications running on that device. You simply select the activity
from the application that you want to profile. Then, hit
the Load View hierarchy button here, and you'll see a representation
of the selected activity's entire view hierarchy represented here in the
Tree View, from left to right. You can see that the
entire hierarchy is also shown here, in the overview and the layout
itself is shown in a wire frame here. Clicking on any view,
at any point within the hierarchy, will show you what is displayed,
showing us the number of views which are contained within the hierarchy
from this point onwards. You can also obtain the performance metrics to
find out how long it takes to measure, layout and draw each
element of the hierarchy. It's a powerful tool and you can find
out more about how to use it to optimize your layouts and
views in the videos and guides linked to from the
instructor notes below. For now, note that we can use
it to find surprisingly deeply nested layouts that need to
be flattened. For example, here we have a layout we
can describe using one of two techniques. One solution is
using two nested linear layouts. But a better solution would
be using a relative layout. The relative layout is one
level shallower. Another tip is to avoid using the frame layout
as the root for layout that will always be inserted
as a child into another one. In these circumstances, the merge
tag is a better alternative. This will be eliminated when the
layout is included into the parents hierarchy. To help you remember
all of these tips and tips, Android has a powerful static
analysis tool called Lint. As you can see here in the
lint window, in addition to the layout warnings we've already talked
about, it also checks from
everything from accessibility problems, missing translations,
and hard coded strings. You can find all the things lint checks
for at the developer tools page linked to in the instructor notes





21 - Responsive Design
======================

Okay. So now we have the wireframes implemented for both screens.
But what does it look like on the tablet? Well, unfortunately,
when we take the phone UI and stretch it out on
a tablet, it doesn't look that great. There's a lot of empty
white space here. We could better take advantage of this screen
real estate by showing more useful information. For example, why do the
details need to be a tap away when they could just
as easily fit on this screen? And, furthermore, if the user wants
to look through the full list of forecasts, their eyes
must go like this, which is an unpleasant reading experience. If
we want the user to read some content, we should keep
the width narrower, so that they can scan it quickly. These
considerations are part of responsive
design. Responsive design means designing your
app by keeping in mind that it'll be used across a
range of different device screen sizes. But how do we do
that, and what does it mean to build for larger screen
devices like tablets. Well, I can show you some examples of
how apps adapt using multi-pane layouts. In the Android Design Guide,
there's a section on multi-pane layouts. One technique is to combine
multiple views together. For example, in the People app, you have
the contact list and the contact card for additional details. On
the tablet, we can put them side by side. This is
also known as the master detail flow. This is the master
list and this is the detail pane. In the settings app,
we have another example. The column width adjust based on the
available screen width. In the calendar app, we have panels, and
they stack vertically in the portrait orientation but horizontally in the
landscape orientation. There also a couple of other examples that you
can look through as well. If you want more information on
designing for tablets or responsive design in general, you can check
out the links below for more details. In this course, we're
learning how to build up the phone UI first, and then the
tablet UI. But in reality, when we're designing
it, we thought about responsive design from day
one. When you're building your own app, it's
bad practice to just completely design and build
your phone app and then start to brainstorm
about the tablet UI. That's because the tablet
UI often has an impact on the phone
design, as well as the architectural decisions made here.





22 - Splitting Devices into Buckets
===================================

So, we need to build an app that adapts well to
all different devices. But there's so many types of devices. So,
where do we begin? Well, we can start by grouping them
into buckets based on physical size. We can have a bucket for
phones and a bucket for seven inch tablets. And a bucket
for ten inch tablets as well. You can go even more granular
based on screen width. But these are the most common buckets
that you'll need. Now, classifying based on size alone is not enough.
Even among devices of the same size, there's still
a wide range of screen densities. Screen density is calculated
from the number of dots per inch on the device
or DPI. Scale starts off with low density devices, also
known as LDPI devices. It has about 120 dots
per inch, then it increases to medium density, high density
all the way to extra, extra, extra high density devices
were the number of dots per inch is much higher.
So, how do we build an app that accounts for
all these different screen densities? Well, when we specify layout dimensions,
we quickly realize that we can't use pixels. For example,
a 48 pixel button that looks fine on an MDPI device
will look much smaller on a higher density device where
the pixels are more compacted and the physical size of 48
pixels is much smaller. The user wants a button to be
easily tappable, and so it shouldn't change based on screen density.
So we need a consistent unit of measure to define physical
size, and in Android we call that density-independent pixels or dips
or dp for short. That way, a 48 dp button will
be the same physical size across all these different screen densities.





23 - Resource Folder Qualifiers
===============================

So, we know we need to create layouts
and assets optimized for different screen pixel densities
and sizes. So, now's a great time to
introduce you to the Android resource framework. All externalized
Android resources, everything from strings to layouts to
drawables and animations are all stored within your project's
res folder. You've already been putting your strings
in the strings.xml file stored in the values folder.
And you've been putting your layouts into the layout
folder. And you know to reference your resources using
at notation within both your xml or within the
code. And then at runtime, Android will insert the
appropriate resource for you. So far, so good. And
here's where things get a little more interesting. Android
allows you to create alternative versions of every resource
by placing them into folders with different qualifiers. We separate
each of those using a hyphen. And we can
add those qualifiers based on anything from language and
or dialect to whether the device is docked, the
type of touch screen, the pixel density of the
display, the orientation of the screen, and most importantly
for a responsive design in particular, the smallest available
screen width which you can support with that layout.
At runtime, Android will check the count device configuration,
it's language, it's screen size, pixel density, everything,
and then load the right layout strings and drawables
accordingly. You can even chain these qualifiers together. For
example, to create a different layout for German language
users to account for all of those really long
German words, or more typically for a combination of
screen size and device configuration. Now, keep in mind
that many of these values can change at runtime.
The most common change being that of orientation. And
it's for this reason that Android activities are destroyed
and recreated whenever the device configuration changes. And that's
because the layout and all of the resources within it
could be completely different based on something as simple
as screen orientation change. Now it's good practice, as
well, to localize your apps and provide translated strings
for all of your users using this mechanism. And that's
a task made a lot easier. Thanks to Google
Play Publisher site that can offer you this service. It's
also a really good idea to provide different drawables
at the appropriate pixel density, so you can get a
nice crisp image on every device. And when it
comes to providing alternative layouts, Android has gone through a
few alternative models. Starting in the early days, with this
screen buckets idea of small, normal large, and extra large.
But, since Android 3.2, the new smallest width qualifier has
given us much more fine grain control over our layouts.





24 - Screen Density Size
========================

Using the screen support page, and whatever technical
specification details you can find on the internet, what
are the sizes and densities of these two devices,
the Nexus One and the Nexus 5? For these
purposes, the screen size should be one of
the available buckets, small, normal, large, or extra large.
And, the pixel density should always refer to one
of our buckets, LDPI, MDPI, HDPI, XHDPI, or XXHDPI.





25 - Screen Density Size
========================

The Nexus One has a 3.7 inch screen, and the Nexus 5, a 4.9 inch display, so
they both fall comfortably within the normal range
for screen size. The Nexus One has a screen
density of 252 dpi, which is hdpi, whereas
the Nexus 5, which was released three years after
the Nexus One, has a much higher screen
density, 445 dots per inch. That makes it xxhdpi.





26 - Images for Different Densities
===================================

Beto mentioned that we should provide bit maps at different resolutions.
In our app, the 48 dib icon on the Nexus S
looks to be about the same size as on the Nexus
that's being displayed is actually a lot bigger in terms of
pixels compared to this one. To confirm, we can check out
the app resource folders. In the drawable folders, we see that
our app has a 48 by 48 pixel launcher icon for
mdpi devices located in the drawable mdpi folder. To
make an equivalent icon for the higher density devices,
we need to make that icon progressively larger. To
know exactly how big to make these images and
what the dibbed pixel conversion should be, we use
an mdpi device as the baseline. This is where
one dib equals one pixel. Then on the HDPI
device one dib equals 1.5 pixels. And then it increases
from there, all the way to an xx HDPI
device, where one dib equals four pixels. For more info
on the conversions, see the link below. Following those
rules, these are the sizes of the launcher icon in
our drawable folders. The MDPI one is 48 pixels
by 48 pixels. The one for HDPI is 1.5 times
the size of this one. From an XHDPI device,
the icon is two times the size of the MDPI
one, and for an XXHDPI device, the icon is the three times the size of this one.





27 - Adding Images to the App
=============================

We're providing you with an asset drop, view the link below,
so that you can download the images and include them in
the app. When we open up the assets ZIP file, we
see that the same icon is provided at different sizes for
different resolutions. In the drawable MDPI folder, we see the assets
that will be used on an MDPI device. In the drawable
HDPI folder, we see the assets that will be used on
an HDPI device. And the same goes for the other folders.
Under the res directory copy over all the asset
folders provided in the download. Clicking on an image shows
a preview of it and the size and pixels
is also shown. At this time you can also remove
the old placeholder ic launcher icons. Our new launcher
icons are located in mipmap folders. These are distinct folders
from the drawable folders. If you build an APK
for a target screen resolution like HDPI, the Android asset
packaging tool, AAPT, can strip out the drawables for other resolutions
that you don't need. But if it's in the mipmap folders,
then these assets will stay in the APK regardless of the
target resolution. But when would you need an image at a
resolution different than the resolution of the device? Well, one example
is the Android launcher app, which controls the home screen as
well as the All App store. The launcher app won't use
the icon at the current density of the device, but rather
pulls a icon for the next highest resolution up. For an XXHDPI device like the
Nexus 5, normally the assets will be pulled
from the drawable XXHDPI folder. However, in this
case the launcher icon will pull the XXXHDPI
version of the launcher icon. That will make
the larger app icons appear sharper on the
all apps screen. In the code replace @drawable/ic_launcher
with @mipmap/ic_launcher because of the new location of the icons.
For the rest of the icons modify the forecast list
and the details screen to display the right weather icon
instead of the placeholder one. See the hints below in
the instructor notes. Using the helper functions we've provided in
the gist below, you can map the weather condition code
to the icon that you'll need. This is what the
app should look like when you're done with this step.
Note that there are two types of each weather
icon, a gray icon and a colored art image.
In the main forecast list we'll be using the
gray icon. However, for the today layout, we'll use the
colored icon. When you go inside the detail activity,
you'll also use the colored icon. At this time,
you can also remove any images you hard coded
into the layout XML, because they'll be populated dynamically now.





28 - Adding Images to the App
=============================

After copying over the assets, updating the launcher icon
in the Android manifest, and adding the Helper methods
to the Utility class, we need to modify the
DetailFragment and ForecastAdapter classes. In the DetailFragment class, in the
onLoadFinished method, we read the weather condition code from
the cursor, then we pass it into the Utility
Helper method to get the colored icon for setting
onto the image view. In the detail fragment on load
finish method, we read the weather condition code from the cursor.
Then we pass this into the Utility helper method, to get
the Resource ID for the colored icon, in order to set
it onto the Image View. The forecast adapter change is a
little trickier because we want to use the colored icon for the
today layout. And we want to use a grey icon for
the other days. In order to distinguish between the other two,
we call get item view type given the current cursor position.
If the view type is today, then we get the weather condition
code from the cursor. Then we get the resource ID for the
colored image from the helper method. And then we set that onto
the image view. If the view type is for a future day, then
we read the whether condition code from the cursor. Then we pass
it into the helper method to get the resource ID the gray
icon and then we send that on to the image view. If
you previously hard coded an image into the layout, then you should remove
that value now. Otherwise, on app launch, it will load
up the placeholder image and then flash to the actual
icon. Now that the wire frame implementation for the phone
UI is pretty much complete, let's look at the tablet UI.





29 - Tablet UX Mocks
====================

When we were learning that response in design, we saw that
a common pattern was to use the master detail flow, which
is what were going to use for sunshine. Here are the
tablet mocks. We have a list of forecasts on the left, and
then for the selected item, we see the detail pane on
the right. This applies for a seven inch and ten inch tablets.
Both portrait and landscape orientations. In portrait mode, the columns are
just a little bit narrower, but it's still a two pane UI.
In terms of implementation, all of this will be under the
main activity. Then on the left, we have the forecast fragment.
And then on the right, we have the detail fragment. On
the phone, we would still have the main activity with the list
of forecast. And then selecting an item would still launch the
detail activity. Here's another way to visualize it. On the tablet, we
have two fragments side by side under one activity. And then
on the phone, we have the first activity containing the first fragment.
Selecting an item will launch the second activity containing the second
fragment. We're going to break this up into multiple coding tasks until
we build up the final layout. First we'll build up the
two pane UI for tablets. Then we'll hook up the communication
between the two fragments, so that's selecting an item replaces the
detail pane on the right. Then we'll learn how to show
an activated state on the currently selected item, so we know
what the details pane corresponds to. We'll also need to make
sure that the scroll position is maintained across
orientation changes. After that, we'll update the detail layout
so it's optimized for these wider screens. And
then we'll modify the adapter so that today's layout
looks like the other days. There's no special
today layout on the tablet. So hopefully it's clear
how we're going to approach implementing this tablet behavior and
we're going to step through it slowly, one by one





30 - Why Do We Need Fragments
=============================

So now you know, where fragments are used and
where we're going to use them in our app. You're
probably starting to ask, why use fragments at all?
If we want to group UI components, couldn't we just create
a View Group or maybe a re-usable XML layout
definition? Yes. But, the real power in fragments goes
beyond grouping UI elements. They allow us to fully
modularize our activities, including the life cycle events they receive
in the app state that they maintain. Fragments were
first introduced in Honeycomb to solve a particular problem. Honeycomb
was the first Android release to support tablets, and
it turned out, the best way for most apps to
create a great tablet UI. Is put two or
more, of their phone activities alongside each other. For example,
if you had a phone app that started with a
list activity like this, which you then clicked an item
would open a detailed activity like this one. What
we call a master detail flow. A good tablet
UI would put them side by side like this.
Functionally, clicking an item on this list, now replaces this
activity on the right, rather than starting a new
one, as it would have done on the phone.
Now unfortunately, Android didn't always support embedding activities within
other activities. At least, it didn't until we introduced fragments.
Now if you just look at the UI elements, you could
be excused for thinking you could achieve much the same thing
using an activity that was built using U Groups and layouts,
without bothering the fragments, which is true up to a point.
But then you'd have to pass through all of the activity
life cycle events, manage the state of each piece of the
UI, keep track of the state of each of the portions
as they changed. And remember, which screen elements were on screen
at any given time, in order to maintain app state.
All of which, is exactly what the Fragment Manager does
for you, when you use Fragments. And that lets you
take a step back and, and treat each fragment as though
it were a mini activity. For example, in the world
of activities, you start one activity from the other, and that
transaction is recorded on the back stack. So, hitting the
Back button, we'll undo that transaction and bring the first activity
to the front. Now the same thing can
happen with fragment transactions. In this case, rather
than starting a new activity, we simply replace
the list fragment with the detail fragment. And then
the back button will undo that transaction and
reverse it. So in theory, you can really take
any act with multiple activities and replace it
with a single activity that's host to multiple fragments.





31 - Why We Dont Only Use Fragments
===================================

Think about how that might work. What are some of the reasons
you may not want to create only a single activity within your application?





32 - Why We Dont Only Use Fragments
===================================

There's a number of reasons you'd be potentially better
off breaking your app into different activities. Having a
single monolithic activity increases the complexity of your code,
making the creation and management of intent filters much
harder and making it more difficult to maintain, test,
and read your activity code. It also increases the
risk of tightly coupling independent components and makes it
much more likely to introduce security risks if the
single activity includes both sensitive information and information that's safe
to share. A good rule of thumb is to create a
new activity whenever the context changes. For example, displaying a
different kind of data, while switching from viewing to entering data.





33 - How Fragments Work
=======================

So if we treat fragments as mini activities, each with
its own independent life cycle in UI, how does that compare
to the life cycle of a real activity? Well as
you might expect the basic life cycle events are much the
same as the parent activity, and as it moves through
the cycles of starts, resumes, pauses, and stops those same life
cycle events will be triggered within the fragment itself. So,
in most cases, you can simply move anything that you would
have put into the activities life cycle handles,
into the corresponding fragment handles. With, of course, a
couple of exceptions. Rather than building your UI here
and onCreate, fragments introduced a new event specifically for
this. OnCreate view is where you construct or inflate
your UI, hook up to any data sources, and
return it to the parent activity, which can then
integrate it into its view hierarchy. There's a corresponding
onDestroy view handler, which is called immediately before the
fragment is added to the backstack, independent of the
parent activity. Now keep in mind that the fragment
manager can add any fragment transactions adding, removing, or replacing
fragments to the back stack, with a single parent
activity's actives. So a fragment can move through this
cycle multiple times independent of the host activity. So
onDestroy view is, where you should clean up any resources,
specifically related to the UI, such as bitmaps in
memory, cursors to data, anything like that to help ensure
that your app's memory footprint isn't bloated by data that's
not needed when the fragment isn't visible. Now as soon
as the fragment is returned from the back stack, onCreate
view is called, and you can re-create the UI and
reconnect data sources before your fragment transitions through the rest
of the life cycle to become active again. And because
a fragment can only exist within an activity,
we also need callbacks to tell us when a
fragment is attached and detached from its parent. OnAttach
is your opportunity to get a reference to the
parent activity. While, onDetach is the last thing that
happens, even after your fragment has technically been destroyed.
Now, the final piece of the puzzle is onActivity
created. This notifies our fragment that the parent activity
has completed its own create handler and represents the point
at which we can safely interact with its UI. Potentially
including other fragments. Now, just like the activity lifecyle we
discussed in lesson three, once the fragment is no longer
visible, there's a chance it will be terminated with no
further code being executed. That can happen, after onStop, in
the case of the activity being terminated while the fragment
is part of that activity's view hierarchy, or after onDestroy
view, if the fragment has already been placed
in the back stack, once the activity is destroyed.





34 - Try the fragment manager
=============================

I've mentioned the FragmentManager, FragmentTransactions in the back stack
a few times. Now, take a look at the documentation
on the Android developer site, and complete the code,
which will replace the contents of the fragment container with
a new instance of the details fragment. Keep in
mind that this transaction should allow the user to reverse
the transaction using the Back button. And in this
case, we'll be calling this code from within an activity.





35 - Try the Fragment Manager
=============================

We use the FragmentManager to begin a transaction, to look that
we want to add this transaction to the back stack, and
then use the replace command, specifying the container ID, whose contents
we want to replace, and the new fragment we want to
put in there. And we execute the transaction by calling commit.
Now you can actually chain another of changes within the same
transaction, so it's also possible to achieve much of the same
effect by first removing the contents of container A and adding fragment
B to that container.





36 - Fragments with No UI
=========================

The final advantage of fragments, doesn't involve user interfaces at
all. As you know, as visual components, activities are destroyed and
re-created, whenever the device configuration changes. Most notably, when the
screen orientation switches. That makes sense, because there's a good chance
we'll want to create a different layout, to better suit the
new configuration. But if we can use fragments to break up
visual activity modules, and find out logical ones as well,
it turns out we can do exactly that. And because these
fragments are non-visual, there's no need to recreate them
every time the UI needs updating. Within the onCreate
handler of your non-UI fragment, call setRetainInstance, passing in
true, and return null in your onCreate view handler.
Then, once the parent activity is created, you can
kick off any connections, threads, or tasks that are
bound to the lifetime of the activity, which don't
need to be interrupted every time the device rotates.





37 - Sunshine Resource Folders
==============================

Great. So, let's apply what we learned about
fragments and resource folder qualifiers to update our Sunshine
app. Figure out which layout folders should contain an
activity main layout file, so that we can accomplish
a one pane UI on phones, and a 2 pane UI on tablets. You can select from
these options for this quiz. You can use the
link provided below to help you answer the question.





38 - Sunshine Resource Folders
==============================

If you open up the documentation, you can see a
couple of examples of typical screen widths. Then it suggests how
you can setup your layout folders, so that you can
have a different mean activity layout for tablets versus for handsets.
And here we define tablets as having a smallest width
of at least 600 dp. To customize the UI further, we
can create a layout-sw720dp folder. That way, we can have a
different main activity layout for ten inch tablets versus for seven
inch tablets versus for handsets. So back to
our quiz. We need to define activity_main.xml file in
our base layout folder so that we can accomplish
this layout on the phone. This layout applies to
both phone, portrait, and landscape, so we don't need
a special version of the layout in the layout
lan folder. We do also need to declare activitymain.xml
in the layout-sw600dp folder. That way it will override
the one pane UI and use a two pane UI in this case. It will be
picked up on devices that have greater than
means seven inch and ten inch devices in
both portrait and landscape mode. And lastly, we
don't need layout-sw720dp because we don't have different
layouts for seven inch versus ten inch devices.





39 - Smallest Width Qualifier
=============================

We mentioned this several times. Put the SW in
the resource folder name stands for smallest width. To drive
home that point, let's walk through a hypothetical situation. Say
you have an app directory structure like this. We have
some layouts defined in the base layout folder and
we override some of those in the layout-sw600dp folder. We
also override one of these in the layout-sw720dp folder. Let's
look at which layouts would be applied on which device.
Say your app is running on a Nexxus five.
The Nexxus five has dimensions 360 dp by six 640
dp. Of the two, the smallest width is 360 dp.
So we use this number to compare it against the
folder names. 360 dp is less than 600 and
is less than, 720 so all the layouts will come
from the space layout folder. For the Nexus 72012 version,
the dimensions are 600 dp by 960 dp. Of the
two sides 600 dp is the smaller width. So you compare
this against the folder names. It turns out that the smallest width
is greater than or equal to 600 dp, so for the
detail and the item layout, we pull it from this folder. Now
the main .xml file is not declared in this folder, so
we fall back to a less specific folder, which turns out to
be this one in the base layout folder. For the Nexus ten,
the dimensions are 800 dp by 1280 dp. Of the two sides,
the categories for all of these because 800 is greater than
most specific folder first. So it will choose this item layout
over these two. Since the detail layout is not defined
here, it will fall back to a less specific folder, which
is this one. Then for the main.xml file, it's not declared
in either of these, so it falls back to this one.





40 - Build 2-Pane Tablet UI
===========================

By taking the knowledge that we learned about overriding
the resources in the other folders we'll walk through the
code together on how to build up the two-pane tablet
UI. First remove the values w820dp folder because we don't
need to provide specific logic for where the current
orientation is greater than 820 dp. Then go ahead and
make the layout XML changes. Then create a new layout
swe600dp folder, and then add a new file called activity_main.
We use the same file and name as in
the base layout folder activity_main, so that this one
overrides the behavior, specifically on tablets. To see the
code for this file, you can check out the gist
below. Essentially it's a horizontal linear layout that can
hold forecast fragment on the left, and the detail fragment
on the right. Now's a good time to talk
about static versus dynamic fragments. In our implementation, the forecast
fragment is a static fragment because we're defining it the XML
layout. No matter what orientation or device size, we know that
we're going to need a forecast fragment in the main activity. On
the other hand, we only need to declare a container for
the detail fragment, but not the actual fragment. It's initialized with
different arguments each time, as a dynamic fragment, so it's better
to dynamically create and add that fragment in a fragment transaction
in the main activity Java code. That way the fragment manager
can keep track of those initialization arguments and pass those
back to us after device rotation. Then we need to update
the one pane UI layouts so that they are consistent with
the two pane case. So in the activity_main file for the
base layout folder, this used to be a frame layout. We're
going to declare it as a forecast fragment, that way it will
match the two pin UI where this is also declared as
a fragment in the XML. That way the main activity never
has to worry about dynamically adding the forecast fragment.
And the main activity onCreate view method, since the
fragment is already inside this layout we can just
remove this so we don't dynamically add it again. Similar,
we've modified the activity_detail layout in the base layout
folder. We change the frame that ID, to be
weather detail container; so that it matches the container
view ID in the two pane UI case,. The pattern
here is that the detail fragment will always be
added to a container called weather_detail_container, both in the two
pane and one pane case. Since we changed the name
of the container we should also update the DetailActivity. This
is only used in one pane mode. Here's where we
change the container name. In the one pane mode, the
DetailActivity will add the DetailFragment, dynamically to this container. After
we modify the layout we should update the main activities,
that we dynamically add the DetailFragment. In the MainActivity
onCreate method, we check for the presence of the weather_detail_container
in the layout to know whether this is a two
pane UI or not. We keep track of this information
in a bullion called mTwoPane. Remember that we start with
the letter M because it's a member variable. In this
case, the bullion should be true. So we go ahead
and create a DetailFragment and add it to the weather_detail_container.
We commit the change by using a fragment transaction, which
Rato introduced earlier. Otherwise, if the detail container is not
present in the layout, then the boolean should be false,
meaning that this is a one pane UI for phones.
In this case, the detail activity will handle showing the
detail fragment. Notice for the two pane case that we
check, if the savedInstanceState bundle is null. If the savedInstanceState
bundle is not null, then we don't create a new one.
And here's the reason why. Say, you want to rotate
the device. Before the activity and fragments get torn down,
we store information in saved state bundles. Then after the
orientation change, the system restores the activity and the fragments
by passing back the same bundle so that it can
be re-created with the same state. That means if the
bundle exists then we should let the system handle restoring
the detail fragment and we can skip this code. Once the
detail fragment is added dynamically make it show some placeholder
data just for testing purposes. Later we'll plumb through the right
logic so that it can display the right information for
the selected date on the left. Modify the DetailFragment so that
it doesn't expect the incoming intent to have the DATE_KEY.
In this case it doesn't start the loader, which is fine,
and it just falls back to some placeholder data that we
have in our XML. The same goes for the onResume method.
We don't restart the loader if the intent is blank.
The reason the Intent could be blank, is because the detail
fragment can now exist within the main activity. So the main
activity doesn't have the same Intent key, that the DetailActivity was
launched with. Once you make the changes for the wireframes,
this is what the app should look like. The reason it
doesn't show an icon here is because we removed the icon
from being hardcoded in the layout. But once this data's populated
dynamically in a later section, it should show up again.





41 - Handle List Item Click
===========================

Once the two panes are showing up on a tablet, let's
modify our code to handle the list item click. On the phone,
if an item is clicked in the forecast fragment, then we
must launch the detail activity. On the tablet UI, once an item
in the forecast fragment is clicked, we notify the main activity,
which goes and replaces the detail fragment. The reason the two fragments
don't talk to each other is because we want to maintain abstraction from
each other. If the forecast list fragment started assuming that the detail
fragment was always right next to it, then the assumption would break.
For example, on the phone where the detail fragment is not inside
the main activity. Therefore our fragment must go through its activity and
the activity must know how to dispatch the event to the other
fragments. Keep in mind that our fragment doesn't always have to be
used in the same activity, in order for the forecast fragment to
talk to the main activity we should create a callback interface. It's
a better assumption to say that the fragment will always be within
an activity that implements this callback than to say that
the forecast fragment will always be inside the main activity. The
detail fragment is a good example of how a fragment can
be used in two different activities. The main activity and the
detail activity. So don't rely on get activity returning a
specific activity class. Using the gist that we provided, use a
callback class to notify the activity that a list item has
been selected. See the hint below on the item click listener.
Then the activity can either launch the detail activity or
replace the detail fragment, based on whether it's a phone or
tablet. When you work on this task, you'll run into
an issue of how to pass the selected date to the
detail fragment. Now your instinct might say to create a
custom constructor where you can pass in the date. However, we
don't normally create custom fragment constructors. If you ever rotate the
device, for example, the system can't call your custom fragment constructor
with the right parameters, however it can use the
empty fragment constructor and initialize it with the same bundle
of arguments you used earlier. So to pass information to
initialize a fragment, create a bundle of key value pairs,
and then set that as the arguments on the
fragment. Don't confuse this arguments
bundle with the savedInstanceState bundle.
Once a fragment has been initialized, you can't change the
arguments. You can only read from them, as seen here.
On the other hand the saved state bundle is for storing information
once a fragment has been running. And you can populate it in
the on savedInstanceState method. The bundle
can preserve state across orientation changes
or if the fragment or activity gets killed by the system. That's why
you receive it back again in the onCreate or onCreateView methods. This
is what the app should look like when you're done. When you
tap on a different date, it updates the detail pane. You can
remove any hard coded data because
the layout should be populated dynamically now.





42 - Handle List Item Click
===========================

In the Forecast Fragment class, we add the callback
that was provided in the gist. Then in the onCreateView
method, we modify the item click behavior. When an
item is clicked, we move the cursor to that position
and then we read out the date string. We
call getActivity and then we cast it to a Callback
object. And then we call onItemSelected, with that date
string. Then in the MainActivity, we implement this Callback interface.
In the onItemSelected method, we fork behavior based on
the TwoPane variable that we defined earlier. If we're
in TwoPane mode, then we create a new DetailFragment
using the date in the arguments bundle. And then
we replace the existing fragment in the weather_detail_container. Otherwise,
in OnePane mode, we create a new intent to
launch the detail activity setting the date as an
intent extra. In the detail activity, we modify the onCreate
method to read the intent extras from the intent that
came from the main activity. We take this date and set
it in the arguments bundle to initialize the detail fragment.
And we dynamically add the detail fragment to this container. In
the detail fragment, we modify the on create loader method
to read from the arguments that were passed in. We get
the date string out and use it with the location
string to build up the Uri to query the content provider.
We also remove any reliance on the incoming intent.
We switch it to use the arguments bundle, instead. For
example, in the onActivityCreated method, we only initialize the loader
if the arguments is not null. In the onResume method,
we also modify the condition so that it checks that
the arguments bundle is not null. When you try it
out on the tablet, there is one bug related to
navigation that we should fix. Say we select tomorrows date.
If you navigate to the settings activity and then hit
the up button, the detail fragment is blank. That's because
a brand new instance of main activity got launched and
for a brand new instance, nothing is selected yet. To fix
this, when the settings activity up button is pressed, we
want to navigate to the previous running instance of the
main activity, where the tomorrow item is selected. And not
create a new instance of main activity. In the settings activity,
overwrite this method. Which the system will call in
order to get the parent activity intent for the up
button behaviour. Get the parent activity intent from the
super class, this should create a new intent to main
activity. Then add the intent flag, called FLAG_ACTIVITY_CLEAR_TOP. This
flag indicates that we should check if the main activity
is already running in our task. And to use
that one, instead of creating a new main activity instance.
Also, this method didn't exist prior to jelly bean, so we need
to add this at target API annotation. If you check the documentation
for the activity class, the getParentActivityIntent method was only added in API
level 16. And you can check that version 16 maps to jelly bean.





43 - Activated List Item Style
==============================

In the tablet wireframes, when a list item is selected
it shows this blue activated state background. This can be done
by setting the background of the list item to be
a state drawable. We can look at the documentation for the
StateListDrawable, which is linked below.
Essentially a StateListDrawable allows you
to specify different drawables based on the state of the view.
We provide you the code for the StateListDrawable. Drop it in
by creating a drawable folder under the res directory. When set
on the background of a list item, the touch selector will
appear transparent by default. As seen here we just see the background
of the activity come through. When the view is pressed then
we see a grey background. Then when the state is activated we
see a blue background. This indicates that it's the selected item
into pay mode. In the list item layout, we set the background
to be this drawable touch selector. We also did the same
for the today layout. In the touch selector file, we also see
that we have some colors defined here. That Android color notation
is for a framework defined color. Our colors are in the colors.xml
file. This is located under the values folder. This us useful
if you need to reference colors multiple times in your app. As
a hint, to make a list item show its activated state,
you can look at the documentation for the XML attribute choice mode
for the list view. Once you find the right attribute, you'll realize
that you want different behavior for one pane versus two pane mode.
While you could change the behavior programmatically in the Java
code, we'd like you to do it purely via the resource
XML files using a style. Read this doc to learn
about how styles can be defined in XML and use what
you learned earlier about defining alternative resources for different sized
devices. In general, styles are a way that you can group
together attributes for a view. For example, these text attributes
are replaced by this style CodeFont. The CodeFont styles is defined
in the styles.xml file, located in the values folder.
You can also have styles inherent other styles by specifying
the parent. There are a couple other examples that you
can check out on this page. This is what the
app should look like when you're done. The two
pane mode shows the pressed and activated state. On the
web pane mode, we see just the press state, because
tapping on an item brings you to the details screen.





44 - Activated List Item Style
==============================

From the documentation, we want singleChoice, mode so
that the list item only shows one selected item at a time.
You can declare this attribute on the list view in the fragment_main file.
However, we don't want to specify ChoiceMode in one ping mode. So, to
have these different code paths, we could copy the file in the Layout folder and
put it in the Layout and put it in the Layout-sw600dp folder.
A better solution is to declare a style. So, we have one layout file but
different styles. In the Base styles file in the values folder,
we declare ForecastListStyle. We leave it empty because we don't need to
specify ChoiceMode. We create a new styles file in the values-sw600dp folder.
In the two pane case, the ForecastListStyle does include the ChoiceMode.
The two files have a different number of styles. If it's not declared in
a higher folder, then it will just fall back to the Base style.





45 - Restore Scroll Position on Rotation
========================================

On the tablet, there's a bug where if you select an item near
the bottom of the list and then you rotate the device, the selected
item is no longer scrolled into view. You have to manually scroll the
list. That means we should store the position of the selected item in
the savedInstanceState Bundle. If the app
is killed, when it's restored, we should
read the position back from the Bundle. If the list isn't populated at
that time, then we should wait for the onLoadFinished callback to use a
position to scroll to the selected item. Check these boxes when you're done.





46 - Restore Scroll Position on Rotation
========================================

In the forecast fragment, we create a position variable. Whenever an item
is clicked, we update the position. Then, in the on save instance
state method, we store the position in the bundle. If the app
gets killed, then we can restore the position from the save state bundle.
This is on the onCreate view method. The reason we store in
a global variable is because the list view probably hasn't been populated
yet. We wait for the onload finish callback to happen when the
cursor is swapped. Then, we can tell the list view to set selection
on that position, and that position will be scrolled into view.





47 - Alternate Detail Layout
============================

We're not quite done yet, with implementing the tablet UI wire
frames. The detail fragment should look like this, but instead it
looks more like this. For the tablet UI, we try to
optimize it more, for wider screens like tablet landscape mode. Another
good place for this layout, is in the phone landscape view,
where there's more horizontal space, than vertical space. For this quiz,
figure out which layout folders, we would need to define the
fragment detail layout in. In order to achieve this layout on phone
portrait, and this layout for phone landscape and tablet.





48 - Alternate Detail Layout
============================

We definitely need to define fragment detail in the
base layout folder, then we override it in the
layout land folder, so that we can achieve this
layout on phone landscape orientation. Tablet landscape view would
work fine because of this folder, but then tablet
portrait would fall back to this layout so we
should just define it in the layout sw600dp folder
so that all tablets regardless of orientation use this layout





49 - Wide Detail Fragment
=========================

The solution of the previous quiz said that we
had to define the wide detail fragment layout in two
places. The layout-land and the layout-sw600dp folder. To avoid having
multiple copies of this file, we should use layout aliasing.
We can check the documentation for layout aliases to see
an example. Say we have a main layout file that's
for one pane UI. In the layout large and layout-sw600dp
folders, we want a multi pane layout. In case you're
wondering, layout large is for backward compatibility purposes for
large devices prior to Android 3.2, that's when the SW
qualifiers were introduced. So back to the point. To
avoid code duplication, we can define the two pane layout
in the base layout folder. Then, in the values-large
and values-sw600dp folders, we can create a reference so that
the main layout actually maps to the two pane layout.
In the Java code, you can reference it as r.layout.main,
but it's actually referencing the two pane layout.
This is what the detail fragment should look
like afterwards. And this is what the phone
landscape view looks like. If you rotate the
device, the phone portrait view should be different.
In the next coding task, we're going to make
the today list item look more like the other items in the list for the tablet.





50 - Wide Detail Fragment
=========================

We build a wider detail fragment layout, using a
horizontal linear layout composed of three vertical linear layouts. This
is what the XML code looks like. And you
can see it linked below. Then we use layout aliasing
to override the fragment detail layout. In the values-land
and values-sw600dp folders, we create res.xml files. In both files,
we alias fragment detail through fragment detail wide. And
now we have a more responsive layout to wider screens.





51 - Today Item on Tablet
=========================

Going back to the wire frames. In the phone layout, we
wanted the today item to be a little bit bigger. But on
the tablets, we just want it to look like all the
other items, because we have an expanded detail pane here. To do
this in the forecast adapter new view method, we should return
the layout for a list item forecast not for the today layout.
Then in the bind view method, for the today item, we
should use the gray icon instead of the colored icon. These decisions
get controlled by the return value of the getItemViewType method. Therefore,
we modify the getItemViewType method so that at the zero position,
if we have to use the special today layout, then we
return view type today. Otherwise, if we shouldn't use the special
today layout, then we should just return the view type for
all future days. This Boolean is controlled by the setter method.
To access this Boolean, we created a public setter method on
the forecast adapter. But who knows how to set this value properly?
Well the main activity knows whether it's one pane,
or two pane UI. So, it can decide whether
we should use the special today layout or not.
We don't have access to the forecast adapter here, but
we can plumb the value through the forecast fragment.
To get access to the forecast fragment, we can ask
the fragment manager and pass it the fragment ID.
Finish changing the code so the app looks like this.





52 - Today Item on Tablet
=========================

In the main activity, we tell the forecast fragment to use a
special today layout if it's one pane mode. In the fragment, we
pass on this value to the forecast adapter, if it's not null.
The reason it could be null is because the activity on Create method
will happen before the fragment on Create View method. And this is
where the forecast adapter is initialized. So we set the Boolean here,
as well. The reason we have this code here is because it's
a public method. So, in the future, it could be called before or
after this method.





53 - Visual Mocks
=================

Hurray, you're done implementing wire frames for the app! Now
comes the visual polish part, where we tweak the styles
and the views to make the app look awesome. Here
are the visual mocks, for what the app should look
like, when we're done. If you work with a designer,
they should also give you red lines. Red lines specify
everything from sizes, font styles, colors, spacing, et cetera. This
should help you achieve pixel perfect layouts that match the
visual mocks. How did we come up with these values?
Well we used the Android design guide, which has information on
metrics and grids, and we adapted them to fit our
app's needs. You can click on the link for more info.





54 - Action Bar
===============

Earlier, we learned about styles and how you can apply them
to an individual View. If you want to apply it to
all of the views in an activity or application, you can
do it by specifying a theme attribute on the activity or
application tag in the Android manifest. Now we style the action
bar to look like these visual mocks. This is the settings,
detail and main activity. For all cases, we want the background
color to be this blue hex color. Then we notice that the
styling of the settings and detail activities look very similar.
They both have the app icon and the activity title. On
the other hand, main activity just shows the logo. It's a
different attribute than the app icon, and this was provided in
asset drop earlier and it's called ic_logo. Because of these similarities,
we can use the same theme for these two activities and
we can create a new theme for the main activity. In
the case of our app, we use app theme here and
we use forecast theme here. We customize the themes in
the styles file in the base values folder. The app
theme inherits from the dark action bar system theme but
we override the action bar style to be our own.
This is defined here. We give it a parent ActionBar
style, and then we specify the background to be the
sunshine_blue color. Remember that this will be used in the
settings activity and the detail activity. Then for the main
activity, we declare the ForecastTheme. We inherit from the AppTheme, in
case there's more attributes that got added here, and then we'll be
able to inherit it for free. But we override the action bar
style to be this one. We set the logo because that's displayed
in the main activity, and we set the display options so
that we can show the logo. For more information on the action
bar display options, you can see the list here. For backward compatibility,
we needed to find these action bar styles in the base values
folder as well in the values V14 folder. The action bar compact class in the
support library handles action bar behavior up
through honeycomb. And then for ice cream sandwich
and above, it goes through the framework
implementation of the action bar. You can see
the code for the two files below. Go ahead and make these two changes now.





55 - Implementing Redlines
==========================

It's time to implement the red lines for the main activity. We
can do one part together so that you get the hang of it.
We begin by styling the list item Forecast Layout. The red line specifies
that the image should be 32 dp by 32 dp. We could hard
code the image view to be 32 by 32 dp, but it's actually
the same as leaving it as wrapped content for height and width. To
know what the icon size would be if we wrap content, we open
up the MDPI version of the icon and we do verify that it
is 32 pixels by 32 pixels. Whether we hard code it
or just add wrap content, visually, the imagine will still look
the same, but in the future it will be more flexible
if we just specify wrap content in case the image size changes.
Next we style the text. On ice cream sandwich and above,
Roboto is the default font for text views. It is created
specifically to satisfy the requirements of UI and to optimize for
high density screens. The red line indicates that the font family is
Roboto sans serif condensed, so we add that to the
code. On the topic of font sizes, having too many font
sizes in your app can confuse your user about the visual
hierarchy of your app, as to what's important and what's not
important. The Android design guide recommends these four type sizes
for consistency with a platform and for user experience. In our
app, we stick to the standard type sizes. And that's an
intentional part of the design, such as the large temperature values.
Hence, when the red line say that the date text
should be 22sp for font size, we could specify Android
text size to be 22sp, but the text appearance large
attribute already maps to that value. Note that the font
sizes are specified in S-P, not dips. S-P stands for
scaled pixels, so you can scale with the device font
size. In our app, we specify text appearance large and
text appearance small, which will give up 22spand 14sp respectively.
The default font color is black, so we don't need to specify
the android text color attribute on our text views. Let's look at the
spacing for the elements within the list item now. We have three children.
The first one is 60 dp wide, with the image centered inside of
it. So we can add a frame layout around the image view
and set the gravity to be center on the image, that way the
image centers itself within the parent. For the other two columns, we shouldn't
hard code such high dp values. Because otherwise the layout won't scale across
other devices. Instead, we can use linear layout weights, the weight of
seven here, and the weight of five. We just tried different values
until we got approximately equal to these values. The trick in doing
red lines is that you want to match the visual specs, but
you also want to be flexible in the code, so that it
adapts to different screen sizes. And this is what the code looks
like for the list_items_forecast layout. We
have a horizontal linear layout, containing
the frame layout with the image view. Then we have a vertical
linear layout with weight seven. And then we have another vertical layout
with weight five. We can check the link below for the full
code file. This is what is looks like when you're done. These
items looks polished, but we haven't gotten to the today item yet.





56 - Implementing Redlines on Your Own
======================================

Implement the rest of the red lines on your own.
Test on the phone and tablet, and in both orientations.
Here's a useful tool to help you with red lines.
Under developer options, you can click show layout bounds. When
you go back to the Sunshine app, you can see
the bounds for all the views, including padding and margin.
When you're done, feel free to check with our implementation
at the link provided below, then check the box to continue





57 - Additional App UI Changes
==============================

As you know, there's always more changes that we wish
we could do, if we just had a little more time.
We're going to leave those as exercises to you, if you want to
keep polishing up. For example, for simplicity, we had all tablet
sizes and orientations use the same layout. However, 7 inch portrait
would actually look better as a one pin UI. So you
can modify your resource folders to make 7 inch portrait use
the phone layout. You can also tweak the two pane behavior
so that the today layout is auto selected when you first launch the
app. Also, this should just say today instead of having the full date. That
way, it'll look more like the other items. As you play around with the
app, feel free to make changes you think would help improve the user experience.





58 - Coding Task on Accessibility
=================================

To evaluate how accessible any app is, you
can step through the Accessibility Testing Checklist. Some of
those steps will involve changing the accessibility settings in
the setting's app. A user will be able to
choose from these options. We have Talkback, Captions,
Magnification, increased font size, and more. I want to point
out one item in particular, Talkback service. Blind and
low vision users can turn on the Talkback service.
It allows them to navigate around the device with spoken feedback
as they interact with each UI element on the screen. There
are slightly different gestures, for example, to select or to focus
on an item. If it's the first time you turn it
on, there will be a tutorial that you can step through.
When the user navigates to these elements, Talkback service will read
aloud the context description provided. Every UI component that doesn't have
any visible text has a content
description. Using Talkback or another accessibility
tool from the checklist will let the sunshine out. Find and fix at least one
accessibility issue in the app. Then please share
what you did in the class discussion forum.





59 - Coding Task on Accessibility
=================================

Thanks for your answer. If you tried talkback on the
Sunshine app, you'll notice that one easy fix to make our
app more accessible is to add content descriptions to our image
views. 'Cause there's no spoken text when a user talks on
it. In the forecaster adopter after we read the weather
forecast from the cursor, we also set it as a content
description on the icon. Similarly, in the detail fragment, once we
get the weather forecast, we set that as the content description
on the icon. Now clicking on the image speaks the weather description out loud.





60 - Sizing Your Custom Views
=============================

Knowing the standard widget library is nice, but as someone who
was once accused as not so much recreating the wheel, as hand
machining my own nuts and bolts, I know there are times
when nothing in the tool chest will quite do the job. That's
when it's time to dive in and build you own custom
view from scratch. Now we're not going to be including any custom
views within Sunshine, but we can still take a look at how
you would build one. We'll start by creating a new class for
our view. Let's call it My View. If
you're building something from scratch, rather than modify an
existing view, it should descend from either view
itself or from surface view. View offers a light
weight, canvas-based approach, while surface view is designed
specifically to support UI elements that require rapid redraws
and/or 3D graphics, using something like Open GL.
It's perfect for views that display video or games.
Now the existing widget library is entirely view
based. So let's take that approach. The base view
class draws an empty borderless 100 by 100 pixel
box. To change that, we override the onMeasure handler,
which allows us to indicate the view size.
We'll also override onDraw, in order to draw our
own custom view content. Now if it turns out
that your view should always be an empty 100
pixel box then you're in luck. Otherwise we need to do
some work. So let's start by setting our view's size. onMeasure is
called when your view's parent is laying out it's children. As
you know, when you add a view to a layout you can
specify a specific height or width, but in most cases you'll
want to either match parent or wrap the content. When a view's on
measure is called by its parent layout, it asks, how much space
will you use? And passes in how much space is available, and
whether the view will be given exactly that much
space, or at most that much space using these parameters.
You can decode those parameters like this. And that'll allow
you to obtain the size and the mode for your
height and width specifications. In this example we're demonstrating just
for the height parameter, but it works the same for
width. We can then use the decoded mode parameter to
find out what size we should set our view to.
If the returned mode is exactly, the view will be placed
into an area of exactly that size. You'll be passed that
value if the layout has specified a specific size or if
the view has been asked to fill the parent. In either case,
it's best practice to simply return the value passed in, unless
that value is below your views minimum size, in which case
you can return the minimum value and rely on the parent
layout to crop or scroll as necessary. Another alternative is AT_MOST. This
indicates that your view can define its own size, up to the
size given. This is typically the case for views set to wrap
content, where the view should be as wide as it needs to
be to display its content, but no wider than that. And of
course all of that's provided that it still fits within the parent
container as indicated by the size
parameter. Now we've just finished calculating
all of that for the height parameter and you'd need to do
exactly the same thing again for width. Once you've done that and
determined the size of your control, you have to call
the setMeasuredDimension method. Here you pass in your width and
height values. If you don't call these setMeasureDimension method, your
app will as soon as your view is laid out.





61 - Create Your Own View
=========================

Now let's take this opportunity to go ahead and create our
own custom view. And we don't need one in particular for
sunshine, so you can go ahead and choose anything. Personally, I'm
a fan of compass control, so that's what I've been building. You'll
need to include three constructors to support your view being created
in code through a resource or through inflation, then simply override the
onMeasure method and set your dimensions. Click here when you're done,
and, you're ready to get started drawing the contents of your gear.





62 - Create Your Own View
=========================

Excellent. Now let's get started putting
some actual content into our custom view.





63 - Draw Your Own View
=======================

Now, it's time to finish creating our new view
by drawing its contents. And, we do that within our
Custom View's onDraw method. Now, the Android Canvas is pretty
standard as far as canvas API's go. It uses the
Painter's algorithm, meaning that each new thing you paint
will cover anything beneath it. The Canvas and Paint classes
offer a variety of brushes and helpers to draw and
fill lines, boxes, circles, and text with colors, patterns, gradients,
and images, as well as offering the ability to move, rotate,
and scale the canvas while you draw. I could spend a lot
of time here detailing exactly how to use each of these tools,
but there's nothing unique to Android and it's covered really well on
the documentation link below. It is possible though to create pretty advanced
UIs this way. For example, for example, if we start by drawing
a circle and then add a second circle and then you just
need to finish drawing the rest of the compass. Here's an example
I prepared earlier. The specifics of what you draw are
different for every view, but one thing that is consistent
is the resource constraint device you're drawing on. And that
the onDraw method be called every time the screen is
refreshed, potentially many times a second; that means any object
created here within your onDraw method, including things like paint
objects, will be created and destroyed at an alarming frequency.
Object creation and destruction can be expensive on Android; potentially
effecting the smoothness of your UI, when garbage collection is initiated.
The solution? Move the scope of any object used within the
onDraw loop, into the class scope. Go ahead and override the
onDraw method within your new view and draw the new control.
In fact, why not take this opportunity to create a custom
control that can display the wind speed and direction fused within
sunshine. Once you're done, you can add it to your layer
using it it's full package name and class name in the XML
like this. Run it and then post a screenshot of your new view into the forum.





64 - Draw Your Own View
=======================

Thanks. Now take the opportunity to browse the forum
and see what other students have managed to create.





65 - Custom View Accessiblity
=============================

So you've got a pretty sweet looking control, but what about
users who can't see the shiny hotness? Accessibility is a key consideration
when building apps and particularly when you're creating new views. You
can start by adding a content description, as you would for every
other view in your layout, to ensure that they are all
accessible. But what about views like our wind speed, and direction gauge,
where the content isn't static? Knowing you're looking at a wind
gauge isn't very useful if you don't know what the speed and
direction it's displaying are. Well, there are couple
of options here. For generic views, which don't control
their own values, this simplest alternative is to
set the content description within your app at runtime.
An even more robust solution is to send
an accessibility event from within your view. Whenever the
visual content has been modified. Then override the dispatch
populate accessibility event, and then the current control's visual
value, the accessibility event. Go ahead and add accessibility handlers
to your view and then click here when you're done.





66 - Custom View Accessibility Solution
=======================================

Perfect. Now our views look good, and are accessible to everyone.





67 - Input Events in Custom Views
=================================

The final step in your custom view creation
is adding interactivity. You can listen for user
input events including key presses, trackball movement, and
most importantly, screen touch events, all by overriding the
corresponding event handlers. Android supports tracking of up
to 11 individual contact points in what we
like to call, jazz hands mode which enables
you to complete complex and multi-finger interaction models. Now,
there's really no interaction required for a wind gauge, but
you can find out more about handling input events by checking
the motion event docs or the developer guide describing how to
create your own, fully interactive custom views, both from the [UNKNOWN]
below. Now, be careful not to let this new found power
go to your head. By definition, your users will have never
encountered your brand spanking new control. So learning how to use
it is going to add friction to the use of your app.
At the very least, it should behave consistently with the rest of the system
and you should avoid creating your own
versions of system controls. If it looks kind of
like a button and works kind of like a button, you should probably just go ahead
and use a button, but I think this non sequitur has gone on long enough.





68 - Lesson 5 Recap
===================

In this lesson, we learned how to create rich, responsive
user interfaces that work across a variety of different hardware
types and screen sizes. We also learned more details on
the Android layout managers and screen widgets and even had to
create our own views from scratch. Join us for lesson
six where we take our foreground-only app and discover how to
make it update and trigger notifications from the background, all
without draining your battery. But first, join me back at Google
for another story from the Android history books.





69 - Storytime Android Open Source Project
==========================================

Andy Rubin, head of Google's Android team in 2013,
famously defined Android's openness with this Tweet, offering the
instructions for downloading and building the Android source code.
As app developers, it's handy to be able to peep
behind the curtains and see how the framework actually
works. And it's an excellent way to find answers to
tricky, Stack Overflow questions. But the Android Open Source
Project, or AOSP as its commonly known, is much more
than that. The source code is available under the
Apache 2 license, so anyone can take it and do
anything they want with it, including modifying it and redistributing
it. And they have. Android has always had a passionate
community of modders, from Steve Kondik and the Cyanogen community
to XDA and many others who have been distributing modified
images of Android for phone owners brave enough to try
them. Even big companies like Amazon have taken the Android
source code and modified it for use in their own
line of products. That openness extends to the apps themselves, the
AOSP includes key mobile apps like the dialer, app launcher,
calendar, and email. And Google offers it own versions of each
as well. In the early days those native apps were
tightly bound to the framework and could only get updated along
with the platform. Which admittedly in the early days seemed like
every few weeks. But as the time between platform releases grew,
so did the efforts to unbundle those apps, allowing
them to be updated through Google Play. That was great
news for users, but it had an even better
side effect for us developers. Because apps like Gmail and
Maps depend on a number of proprietary Google APIs,
Google released Google Play Services. Also code named after food
to support the Google apps, while also giving us
developers Google specific APIs, like Maps, Wallet, Drive, and YouTube.
So now we get the latest and greatest Google specific APIs
every six weeks or so. And rather than being tied to
a specific platform, they're available on every device, back to Gingerbread.
Oh, and of course every native app is also entirely replaceable.
So you can publish apps to replace any of the apps
shipped with the device. And we can build our own SMS
apps, homescreens or maps apps, publish it on Play and give
users the choice to make it their default. When Android was first
announced, Andy Rubin described the AOSP as, all the software
to run a mobile phone but without the proprietary obstacles
that have hindered mobile innovation. Today, Android runs on more
than just mobiles, but the core of its purpose remains
the same. A platform and ecosystem that allows models, carriers,
OEMs and app developers to innovate and compete. So, armed
with an open, evolving platform enriched by a growing set
of Google API's and the ability to create great apps
that let users customize every aspect of
their user experience, you can use other people's
work to help define the computing platform of
the future. And that's my definition of open.
