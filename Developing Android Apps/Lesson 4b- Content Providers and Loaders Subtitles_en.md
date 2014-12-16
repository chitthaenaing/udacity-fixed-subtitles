
01 - Content Providers
======================

Now that we have a database, we can take
advantage of an Android pattern to bind our view to
our data model known as a content provider. A content
provider allows us to think of our view data in
terms of URIs which is a convenient structure for
our applications. We can have views display different data based
upon which URI is currently active. As it turns out,
a URI is the primary data member for those Intents,
that Rato so gloriously described. With a combination
of content providers and intents, one can nicely
decouple the data being displayed from the view.
And there's one other great thing we get from
using URI with a data location. It's easy
to have our database notify components that are registered
to observe that location which causes our cursor
to update our list and show the latest data.
Content providers can return all sorts of data but typically, they return either
a list of items, or a single item. Here's how one might modify our
query to select the data for a specific date, rather than a range of
days which in this case also contains more information for the viewer to see.





02 - Why Content Providers Matter
=================================

Before we go ahead and build a content provider
you wouldn't be out of line to ask, well why.
The simplest answer is that it allows you to
share your data safely and efficiently across app boundaries by
abstracting the underlying data source, be it SQLite like
this or files or really anything else. So that other
apps can access it without really needing to understand how
you stored it. In fact, the calendar, SMS, and contacts
APIs work that way, using shared content providers.
We're going to share our weather database later, but if
you're not planning to expose your app's data, you're
probably thinking you can skip this bit, right? Well,
almost, in a lot of cases you could,
but really shouldn't. For example, in Sunshine we're using
SQ Lite. But you could be storing data in
files, dynamic run-time data or even just a different
database library. By using content providers, it's easier for
you to potentially switch out the data source and
much easier for someone other than you to manage
the UI layer code without them having to understand
the depths of your data storage implementation. On the
UI layer, it's a generic mechanism that returns cursors.
The same of those returned by SQLite databases. So,
if your data layer implementation changes, then your content provider
is effected. Still, it's just you writing the code right
now and that's a lot of boilerplate for the sake of
following a neat design pattern. Well, keep in mind that
as far as the framework is concerned, all data is handled
through content providers. So, if you want to interact with
anything outside of your app, such as sending data to a
widget or returning search results from the newer app, you'll need
a content provider for that too. In fact, that's how the
Google play store and Gmail widgets work. As well as the
ability to get search results from Google Play. Similarly, there's a bunch
of APIs designed to optimize the process of synching and querying data,
and updating UI accordingly. And all of them also expect content providers.
That includes sync adapters and cursor loaders. Which make your
app able to efficiently sync with your server, load data in
your UI layer, and which include built in content observers that
will update your UI automatically when the underlying data changes. You
could, of course, build all of that yourself but at
a certain point the advantage you gained by not writing a
content provider to begin with is lost in the process of
having to recreate all of the useful clusters that utilize it.
We'll take a look at publishing your content provider and using Lotus
to access it efficiently later in this lesson. And we'll explore sync adapters
in lesson six when we look at doing efficient background updates. But
first, Dan is going to show you how to actually build a content provider.





03 - Creating a Content Provider
================================

These are the required content provider functions. You
can implement them any way you like, but
you'll find that it's really straight forward to
implement them on top of a SQLite database. The
parameters packed into our content provider functions match
the parameters used for Android's SQLite's interface almost
exactly. The biggest difference is that we replaced
the table string, with a uri. In the simplest
content provider implementation, the query is passed
straight through to the database. Note that
there are parameters that cannot be easily
controlled through content providers, such as grouping functionality.
But the interface allows for very flexible querying.
Most importantly, the projection or columns allows for querying
to be much more efficient. Because the column
indices and our cursor object match the projection that
we submit to the database, we can just
get the values by their numerical index instead of
having to use getColumnIndex to look up each
column index. All right, it's time to code some
more Sunshine. Let's code a content provider.





04 - Adding Content Provider to our Contract
============================================

Before we start creating our content provider,
it's time to go back to our weather
contract. We're going to use the weather contract,
for both database definitions and content provider definitions.
These are the URIs that our contract will expose for a data and view. Some of
them return a list or directory of items.
And some of them return as single item.
The first part of the URI is the content authority which is how the system
disambiguate URIs from different application, similar to the
relationship between a domain name and its website.
A convenient string to use for the content
authority is the package name of the app.
Let's return to our weather contract. We'll add
the CONTENT_AUTHORITY and the BASE_CONTENT_URI for our content
provider to the beginning of our weather
contract. Next, we'll add strings to the base
path of our URIs. Each URI essentially points
to a different table. For each of our
tables, location entry, and weather entry, we
create a content URI that represents the base location for each table. Now,
we'll add some magical value to the location and weather entry contracts.
These contain special MIME type prefixes that indicate that
the URI either returns a directory, list of items,
or a single item. Now, we're going to add some
URI builders and decoder functions in weather entry. It's
convenient to have this as it makes fewer places
in your code aware of the actual URI encoding,
keeping this knowledge in the contract. If you're just
going to have a URI with a standard integer
primary key, which is a typical way of distinguishing between
a query for item or the query for a list
of items, one can use the content URIs with appended
id function. Strings, such as a location setting, can be
appended with the append path function. We can also these
functions to add potentially useful query parameters. In this case,
we use a query parameter for the start date. Query
parameters are useful for when we have a fixed database
query that we want to have some limited parameterization
for. In this case, it will be a parameter for
our join between the two tables. Finally, we have
this function that builds a two part URI that contains
both location and date segments. Below this, we have
helper functions that help decode the URI structure. This hides
the URI structure form our code as well and
places all the knowledge in one place within the contract.





05 - Adding LocationEntry with ID UriBuilder
============================================

Add code to build a URI for querying the
location by ID within the LocationEntry part of the WeatherContract.





06 - Adding LocationEntry Solution
==================================

All right, you're done! Let's look at the solution. This exercise
is pretty short. And it looks just like the way we
did the build weather URI, earlier. Once again, we just
use content URIs with appended ID, to append the ID of the record.





07 - Create WeatherProvider
===========================

Let's create a weather provider class within the
data package. We'll have it extend content provider. [SOUND]
You can have the required method by heading
Ctrl+I once more. Content providers implement functionality based upon
the URI's. Each URI type is tied to
an integer constant. Our content provider will implement five
different types of URI's. Each one of these
URI's will be used for different types of queries.
For now, lets just add these constants into our weather provider class.
Android provides a URI matcher class to help
match URI's to our integer constants. It provides
for an expression syntax to match various URI's.
Now these are just examples. You're going to have
to implement one of these very soon. Note,
that there are two wildcards. The pound sign
is used to match a number, while a star is used to match a string. Using this,
you can very, very easily match any of the URI's
that we would want to pass in to our content provider.





08 - Write the UriMatcher
=========================

In the previous explanation of our content URIs,
I showed the five URI types we would use
in this app, fill out the build URI
match function. Note that you cannot test this, yet.





09 - Write the UriMatcher
=========================

All right, you're done. Let me show you my solution.
We start by creating an empty uri matcher. I don't want the root node to match
anything. This is typical behavior. To make the
code easier to read, I'm making a shortcut
to the weather contract content authority. Technically, date
will always be numeric, but I'm matching the
paths with star, because they are stored in
our database as strings, just to be consistent.
Finally, we have the location URIs. Since the ID in the database is
always a long integer, it makes perfect sense to use the pound sign pattern.
Now we can add our class variable. [BLANK_AUDIO}





10 - Write the UriMatcher
=========================

All right, you're done. Let me show you my solution.
We start by creating an empty uri matcher. I don't want the root node to match
anything. This is typical behavior. To make the
code easier to read, I'm making a shortcut
to the weather contract content authority. Technically, date
will always be numeric, but I'm matching the
paths with star, because they are stored in
our database as strings, just to be consistent.
Finally, we have the location URIs. Since the ID in the database is
always a long integer, it makes perfect sense to use the pound sign pattern.
Now we can add our class variable. [BLANK_AUDIO}





11 - Coding the Content Provider
================================

Before we start coding our content provider, let's
make sure the manifest is set up correctly.
You need to update the androidmanifest.xml file within
the application tag to add the content provider. So
that android's content resolver can see it. The
authority really should match your package name. While
the name indicates which file your content provider
class is in. Next, let's code up on create.
We'll begin by creating an instance variable for our
database helper. In on create, we initiate our instance variable.
We return true, which is how we tell Android that
the content provider has been created successfully. It turns out
that we can repurpose the same test we did
for the database to test our new content provider. This
will allow you to both test your content provider code
and see how the content provider calls replace the SQLI
database calls. Let's start by using Android
Studio to copy TestDB to test provider.
Let's rename test create DB, to test delete DB.
So we can start with a clean slate. We'll just leave the M context dot delete
database line in there. We'll leave the other
test the same, we'll get to it soon.





12 - Coding ContentProvider getType
===================================

Let's go back to coding the content provider. The first
of the required content provider methods we're going to implement
is getType. getType is used to return the mime type
associated with the data at the given URI. We use
the URI matcher we built earlier to match the given
URI against the expressions we've compiled in. For each match,
we return a unique mime time, that starts with either
V and D Android cursor item, for a single record or
V and D Android cursor dir for multiple items.
We've nicely already defined these strings in our WeatherContract, in
our tests, we're getting the type. We're going to compose
URIs, using the standard methods we have, inside of our
WeatherContract. Then, we're going to pass these in to
our content provider, through the ContentResolver. Which is how you
call all functions in content provider. And we're going to
make sure it matches what we have in the contract.
I've placed the actual values as comments so you can
see, really, what's going on. Note that some of these returning
type directory, which means they're going to return a list
of different items in their cursor. While others are returning type
item, which means they will only return a single item.
Next, we're going to have you finish up getType. You're going to add
the appropriate cases in this switch statement. But to return mime
types, the remaining URI types that we aren't yet handling. No,
there are only two to add, and they
really look just like the ones in WeatherContract.





13 - Coding the Content Provider query
======================================

Let's continue coding our content provider. Query will be
the most complex of the required content provider methods.
We begin with the bones of the query operation.
We use our SURI matcher object once again to
switch on the type of URI. This is the
only function where we will have to fill out
every type of URI in the content provider. Several
of them are used for querying only. We'll implement
the basic weather query first. For this query, we
just pass through all of the parameters to the
database. Pretty simple stuff. Let's fix these parameters so
they make some sense. So the strings here is
actually a projection. S is a selection. The next
array is selectionArgs. In the last string is sort
order that matches a lot better. At the bottom
of the function, we're going to set the notification
URI for our cursor to the one that was passed into the function.
This causes the cursor to register a content observer. To watch for changes that
happen to that URI and any of its descendents. By descendents
I mean any URI that begins with this path. But, we still need to test this.





14 - CodingContentProviderqueryTestDELETE
=========================================

Let's update the test insert redv test to use
to use the weather provider to get the weather
query. We'll start by changing the name to test,
insert read provider. So, all we need to do
is take this dv query, and replace it with
the equivalent content provider one, which gets the content
resolver, and then queries the content URI. Now, the
content provider one doesn't take a few of these parameters.
For example, it doesn't take TABLE_NAME, because that's implied
by the URI, nor does it support group by.
The rest of it should work pretty well. Now,
let's run the test and verify that it passes.
[BLANK_AUDIO]
And the test still passes. [LAUGH] Again, it's very easy to substitute
out something that calls directly to the database with a content provider.





15 - Implement Location and Location-ID
=======================================

Implement the queries for location and location ID in
our content provider. Note that content URIs contains a
function to get the ID from a standard URI that contains an ID on the end of it.





16 - Implement Location-ID Solution
===================================

All right, you're done. Let's go look at the
solution. Where we left off last time as you can
see we have two empty queries for LOCATION_ID and
LOCATION. These two queries look a lot like the queries
we have for weather. For LOCATION_ID this looks almost
exactly like what we do for weather. Except we use
a hard coded query, rather than relying on the one
passed into the function. Since we know that this is
always going to be a long integer value, it is
easy to just build the value into the query string. Location,
on the other hand, looks almost exactly like the weather
query. We just pass all the parameters into the database. You
didn't think you're going to get away without testing, did you?
Don't worry, it's really easy to add this test. It turns
out that calling our function within the provider, works almost
exactly the same as the way we did the weather table.
All we have to do is replace WeatherEntry.CONTENT_URI with
LocationEntry.CONTENT_URI. And of course we still have to delete the
two columns that we can't support. Now that will actually
work. So, let's test that to make sure that works.
And our test passed, as expected. The interesting thing about
this test is that it's just only testing the first
query and it just happens to work because we deleted
the database before the start. It'd be much more interesting
to query for the row that we just inserted. And this will test the
other query. Now, let's run the test again. And those also passed. So, we have
some of our queries from our content
provider functioning. But, unfortunately, we still have
to do some of the more complicated
things in the provider. Let's talk about joins.





17 - Add a Join query
=====================

When we first defined our tables we defined
the relationships in terms of constraints. Now, we
are going to implement that relationship into our
query using a join this join will allow us
to query the weather table for values from
a specific location setting. We'll start by adding a
SQL Ite query builder class variable at the
top of weather provider. We'll initialize the SQLI to
query builder in the static constructor for the
class, describing the join between both tables. Note,
since both tables have a column named _ID.
We must explicitly use the table name in
order to disambiguate which underscore ID we are
talking about in the join. Then we can
define the query. The query is to find
using the question mark replacement syntax. These question marks
will be replaced by the query parameters. Since we
allow people to specify the start date in the URI,
we're also going to add a second selection, which includes
checking to see whether date text is greater than or
equal to our parameter. Next, we'll add a method to
get the weather by location entry, using the same query
builder. Note, that we fetch the parameters from our URI.
And though the string arrays, they can be substituted into
our query. Note that if URI does not contain a start date we actually use
a different selection. Finally, we add the function
into the query routine of our content provider.





18 - Testing our Join
=====================

Now we have to test this. Let's go back to
test provider once more. It'll be helpful to parameterize our tests
a little bit more, both for location and for date.
All right, now that we've parameterized our tests, let's go back
at looking into our insert read provider. For the weather,
we're currently using the raw content URI to query. But we
can also query on additional data. By cutting and pasting
this text, we can try the text with different query parameters.
We'll close the cursor because it's good
practice. Now for our second weather query,
we're going to test the first of
our new URIs: WeatherEntry.buildweatherlocation and we can use
our new task location parameter. Alright, let's
try running this test. And our test
passed. Alright. We're going to add one more
test with a slight variation on the query
using build weather location with start
date, instead. So, we're going to use test
location as well as test date. And now, let's run those tests. And
our tests pass. Now, sometimes it's
useful to actually demonstrate a test failure.
Let's put in a date that's certainly going to fail, 20150624. We would
expect this test to fail. Let's try it and indeed it fails. So
we're pretty sure that our tests are actually doing what we expect them to.





19 - Add the Other Join query Quiz
==================================

All right. You know what time it is. Time to implement the query
for WEATHER WITH LOCATION AND DATE in our content provider. Remember, we defined
functions to get the location query and date from a URI in our
weather contract. Also, write a test to make sure you got the answer write.





20 - Add the Other Join query Solution
======================================

All right. You're done. Let's look at the solution. All
right. We'll move to WeatherProvider. We start by creating a
third selection query up at the top of our provider,
much like the second query except that we search for
the exact date. Rather than a greater than or equal
date. We then add a function that uses the weather
contract. Functions to get the date and postal code from
the URI. We use the new query we just wrote.
And the same SQLI query builder we used before, since it only defines the join.
And then we just call this function in the
weather with location and date switch statement in our
query. That's it. Those are all the queries we
plan to use for Sunshine, but we still need to
test. Let's go back to test provider. As you
can see, our test has a very, very good model
already. Let's fix up some of these old tests
and close the weather cursor. Now, we can simply copy
this test over. It turns out that the exact
same data is required for our new URI. All
we have to do is to build weather location
with date, instead of saying weather location with start
date, and we should get the same result. Let's
see if that's really true by running our tests.
[SOUND] And our test passed. So, we've now implemented
all of the queries, but we're not done with our
content provider yet. After all, we're not writing
any data into the database through our content provider





21 - Add the Other Join query Solution
======================================

All right. You're done. Let's look at the solution. All
right. We'll move to WeatherProvider. We start by creating a
third selection query up at the top of our provider,
much like the second query except that we search for
the exact date. Rather than a greater than or equal
date. We then add a function that uses the weather
contract. Functions to get the date and postal code from
the URI. We use the new query we just wrote.
And the same SQLI query builder we used before, since it only defines the join.
And then we just call this function in the
weather with location and date switch statement in our
query. That's it. Those are all the queries we
plan to use for Sunshine, but we still need to
test. Let's go back to test provider. As you
can see, our test has a very, very good model
already. Let's fix up some of these old tests
and close the weather cursor. Now, we can simply copy
this test over. It turns out that the exact
same data is required for our new URI. All
we have to do is to build weather location
with date, instead of saying weather location with start
date, and we should get the same result. Let's
see if that's really true by running our tests.
[SOUND] And our test passed. So, we've now implemented
all of the queries, but we're not done with our
content provider yet. After all, we're not writing
any data into the database through our content provider





22 - Coding the Content Provider Inserting
==========================================

While it's great that we can query our database
through the content provider, it would be nice if
we could also use the content provider to put
data in. We'll begin with the insert function. Let's fill
the insert function with the same URI matcher code
we had in the other content provider functions but with
one change. We're only going to match the base
URIs. There's a good reason for this. When we insert
into our database, we want it to notify every
content observer that might have data modified by our
insert. It turns out that cursors register themselves as
notify for descendents which means that notifying the root
URI. We'll also notify all descendents of that URI.
If we were to notify based on anything else
other than the root URI. Then a cursor listening
on the root URI will not get notified of a
change that would certainly impact it. So we have to
be very careful when doing that. For this reason it makes
a lot of sense to only allow insertions at our
root URI into our database. That way, it's very, very easy
to handle notifications. It means that we also don't have
to build a combination query for our insert. Containing a parameter
coming from the URI. With the rest of the parameters
coming from a function. So for weather we just pass the
parameters into a database insert call. We should
throw an exception if the insert fails. The
only trick here is to make sure we
return the correct value, which is a URI. Fortunately
we made a function to build these URIs
which contain the weather path followed by an
ID. Let's go task this. Once again, we
go to test provider and we modify our test.
Because after all it's supposed to be test insert
read provider. Here's our insert statement. Obviously it's still talking
to the database. We're going to want to make it
talk to the provider. As always, that's pretty straight forward.
We always get our provider by using our content
resolver so we can replace this insert statement with the
db with a content resolver instead. And of course
we don't use a content resolver against a table name.
Instead we do it against the WeatherEntry.content URI.
But there's still something wrong. Of course, we
don't actually return the weather row ID, we
return the URI. But we can still get a
weather row ID. How do we do that?
Quite simply. Using a helper function in content
URI's, and we don't really need that insert.
After all, we know that this content resolver function
isn't actually going to return unless the value is true. Now that we
finished that, let's run the test. And make sure that we can actually
insert using our content provider. And our test passed. Well, you know what
time it is. Time for you to write some of this as well.





23 - Coding ContentProvider Finish Inserting
============================================

Implement the rest of the insert content provider method. Handle
the location URI. And notify any registered Contentobserver of the
change. It might help you to know that you can
use getContext() getContentResolver() .notifyChange(uri, null)
to notify any registered observers.





24 - Coding ContentProvider Finish Inserting
============================================

Adding support for the location insert, looks almost
identical to adding support for weather. We just have
to select the right table name, and return
the right location URI. But there's one more thing
to do. All we have to do is
call getContext().getContentResolver().notifyChange on the
URI that was actually passed
into our function. To notify any observers that need
to know that UI has changed. And that's it.
We've now finished insert. However, of course,
we're never done without also adding a test.





25 - Coding ContentProvider Finish Inserting
============================================

Adding support for the location insert, looks almost
identical to adding support for weather. We just have
to select the right table name, and return
the right location URI. But there's one more thing
to do. All we have to do is
call getContext().getContentResolver().notifyChange on the
URI that was actually passed
into our function. To notify any observers that need
to know that UI has changed. And that's it.
We've now finished insert. However, of course,
we're never done without also adding a test.





26 - Coding the ContentProvider Testing
=======================================

So once again we're going to replace the one
remaining part of our test, that still has a
standard database. Let's actually get rid of our database.
That leaves us with this lonely insert statement. Once
again, like with everything else, we're going to replace that
with a content resolver call. And of course we're
actually not going to be inserting the table. We're going
inserting the content URI. Getting rid of the nulls.
Of course this doesn't return a location row ID.
That's alright. We can pull it from the URI. Using
the helper function from content URI's, we can leave
that insert in there, although it's not necessary. And the
rest of everything should pretty much be as it
always is. We're no longer using a database at all
within test provider, but entirely using the content provider.
Let's run our tests and see if they still work.
[BLANK_AUDIO]
And they all pass. Congratulations, we now
have a fully functioning content provider that can
both insert and query. But, of course,
there are other things you might want to do
with the content provider. Now, we're not
actually going to use any of these things
in Sunshine. But it's really important to know.
So, I suggest you actually implement these functions.





27 - Updating and Deleting
==========================

The update and delete methods look a lot like the
insert method, except they update and delete and neither actually returns
a URI upon completion, but instead, the number of rows affected.
Also, make sure to notify our ContentObservers while you're at it





28 - Updating and Deleting Solution
===================================

All right, you're done. Let's take a look
at the solution. Delete is actually pretty straight forward.
Note that once again we have almost useless
parameters. S actually refers to our selection while our
strings here are actually our selection Rs. One
little trickiness in delete is that if you actually
put no selection in, it'll delete all the rows.
I still want to make sure we notify the change.
I've done the slide optimization here. It would be
fine to just notify the change always, but I've decided
if no rows have been deleted, then we should
bother not notifying. Unless the selection is null, in which
case we deleted all the rows, but again, it
would be fine just to use notify change here even
if no rows are actually deleted. After all, the intention
was that rows would be deleted. Once again, let's take
pity on someone who might have to maintain your code, and change
these default parameter names to something
more useful like selection and selectionargs.
Other than that, update looks almost exactly like delete. Once
again, we are returning the number of rows impacted by
the update, and we don't bother notifying if no rows
were impacted. Now that we've finished writing update and delete, we
can go back to test provider and add some tests.
First, let's remove test delete.db, the last test that doesn't depend
on the provider. Instead, let's have a test that deletes
all the records and checks to make sure they're actually deleted.
Remember, passing a null query actually deletes all of the
records in a table. So this is really straight-forward, just passing
nulls. Since we're depending on the database for our deletes, it's
not all that particularly important that we actually test the delete
functions, although it would be nice to add that later on.
The reason we'd want to add it is to make sure that
our constraints are valid. And once we're done doing the deletes,
we check to make sure that there are no rows left.
Note that we have to delete weather entry before we delete location
entry, because you've got constraints in the database that prevent weather entry
from existing if location entries do not also exist and would also
prevent the deletion of those entries. So let's check to see whether
that actually works. All right, that passes. Good news. Now we can
add the same test to the end to make sure we can
delete all of the rows at the end and it still works.
And as you can see, they still pass. So now we know we've
deleted records before and after running tests. Now, let's try
that update. We'll do just an update of the location.
Again, let's start by deleting all records and inserting a
record that we want to update. One of the nice things about
content values is you can actually copy them using a
copy constructor. After that, we can add the ID we
want to update and then put the name of the city
we want to update it to. In this case, from North Pole
to Santa's Village. And then, finally, we call update.
Assert that we've actually updated exactly one record as
we'd expect, and then do a query on that,
validating our cursor, making sure that our update actually works.
And then finally, we close our cursor. It all
looks pretty good. And let's see that it actually runs,
and does the right thing. And our tests pass.
So, we've now proved that we can update, delete, insert,
and query and all this is fully tested.
In other words, the bones of our application
are finished, just waiting to attach our UI.
Let's go through and attach some of our UI.





29 - Efficient Updates Inserts
==============================

Well we're done with the core of our content provider,
but there's still one more optional method that'll make things
much more efficient. We'll start by hitting Ctrl+O to look
at the functions we can override. It turns out there's
a function called bulkInsert. Anyone out there who has done
database work knows that putting a bunch of inserts into
a single transaction is usually much faster than inserting them
individually. Bulk insert allows us to do just that. The default
implementation just calls insert a bunch of times, but
we can wrap it in a transaction if we implement
it ourselves. We're only going to add support for
weather forecast transactions here, since they're the only we insert
in bulk. We start by calling db.beginTransaction. For each
ContentValues passed in, we insert and update the number of
records inserted. When we're done, we set the transaction
to be successful. Otherwise our records will not be committed,
and then rely on the finally statement to end the
transaction. In the default case, we just call the super
class. Remember, it does the insert, just not optimally. And
that's it, that's all we have to do to implement bulkInsert.





30 - Inserts with the ContentProvider DELETE
============================================

All right. Let's get serious now. We need to move to using a real task to fetch
the weather. After all, weather is serious business. Let's
create a separate FetchWeatherTask to populate our content provider
database. And we'll start by moving our existing
task over to this new one. Wow. It's good
to get that big task out of ForecastFragment. And
we're going to change the parameters to string void, void.
Oh, we've got a lot of compile errors to fix. That's okay. For
now, we're going to delete everything except for the do in background part of
the async task. Let's give it a constructor and pass in a context.
[NOISE] In doing background, let's save off the location
query as a string to make the code easier to understand.
And for fun, let's fetch the maximum 14 days of
forecast data. Since we temporarily deleted get weather data from JSON,
we can remove from the try catch at the bottom. We're
going to look for lots of additional data from the openweather API.
To make this easy, here are the strings we'll
use. We'll also get the city name, the latitude, and
longitude returned by openweather. Now, we do have to wrap
this in a try, in order to make sure that
we handle the exception. Alright. Now we've handled the
exceptions so everything compiles. And hey look, we've got the
city name and coordinates. Hm, we've now gotten the location
that we can use to insert things into the database.
And we haven't done this in a while, so it's time for a programming exercise
but before we can do that let's actually
make sure that your app can really compile.
Let's go back to Forecast Fragment, and take a
look at how Fetch Weather Task is working. One
is that Fetch Weather Task is going to require that
we get an activity or some other context, and pass
that in there. Another thing we can do is,
remove these pesky shared preferences. And there we have
it, a utility function that gets us our preferred
location. Alright, so now, we're ready to run the applications
we can actually do the next exercise. Once again, we're going
to be calling in to our very new fetch weather task,
but it's not going to do anything but try and read some
Jason. So we want to make it do something beyond that. [SOUND]





31 - Inserts with the ContentProvider
=====================================

All right. Now for the fun part. Code the method below
to insert the location into the location table if it doesn't
already exist. Return the ID of the location. Now you can
do this the easy way using a query followed by an insert.





32 - Inserts with the ContentProvider
=====================================

All right. You're done. Let's take a look at my solution. All right. So, here's
what our addLocation function looks like, locationSetting, cityName,
latitude and longitude. Just like in our content
provider test, we can use getContentResolver to query
based upon the content URI. Remember that the
default location entry CONTENT_URI passes all of the
parameters into the database. So we can easily
construct a query containing the location setting to see if
it's in the database yet. If it's not, then the query
will return an empty set and we should insert the new
city name, location setting, latitude, and longitude into the database together.





33 - Finishing the FetchWeatherTask
===================================

As you've noticed, we aren't actually testing
that new function yet. Fortunately, we have a
pretty good spot in doing background to
do that. After all, right here we've already
parsed everything we need to call that function. So, we can just call long
locationId equals addLocation cityName locationQuery
citylatitude, and citylongitude. It would be useful for our contract to have
another method, one that converts a date object
to the format our database uses. Note that Android
Studio may have trouble importing this data object,
since Java has two day classes, one in databases,
and one in java.util. We want java.util.date. Back
in the FetchWeatherTask, we're going to collect the data
from JSON into the weather array. All right, so
we now have a whole bunch of data that
needs to be inserted. Guess what we're going to do next?





34 - BulkInserts with the ContentProvider
=========================================

All right. Now we're back to interacting with our ContentProvider, so it's time
to have another exercise. Use the
ContentProvider to insert the vector of content
value objects into the database. [LAUGH]
Don't worry, we're really almost done with
inserting data. Next is the fun part that makes this all worth it.





35 - BulkInserts with the ContentProvider
=========================================

All right. You're all done. Let's go
over the solution to bulkInserts with the ContentProvider.
While we cannot insert a vector directly, we can easily convert a vector into
an array. Once we've done this, the
the bulkInsert method will insert very efficiently. Now,
our backend will update efficiently. But we
really need a way to update the front
end without introducing framerate jitter. Fortunately, Android
offers a pattern for that known as Loaders.





36 - Loaders are Awesome
========================

Let's talk about Loaders. Loaders are awesome. They were
introduced in Honeycomb, but are available as part of the
support library. So you can take advantage of them
when supporting earlier platform releases. Loaders are essentially the best
practice implementation for asynchronous data loading within an activity
or fragment. So when you create a Loader, it creates
an async task to load data on the background thread.
It then syncs with the UI thread when the initial
loading is complete, and can be set up to monitor
the underlying data, and deliver any updates to the UI
thread as well. Best still, all that work you did
adding a content provider to your database pays off right
now, with the cursor loader. The cursor loader is an
implementation of the async task loader, specifically designed to query
content providers, and return a cursor, which you can then
bind directly to a UI. It will automatically update that cursor,
whenever the underlying content provider changes, based on changes
to the underlying database. And it will reconnect to the
last returned cursor after being recreated, along with the
host activity, after a configuration change. That means you won't
have to requery data, just because the device was
rotated. So the cursor loader handles all of your cursor
management and background thread creation, UI thread synchronization, and data
source monitoring. If you chose not to use content providers,
you chose poorly. But still, you can take advantage
of loaders, you just need to create your own
loader by extending a think task loader directly. You
can find out more in the instructor notes below.





37 - Disadvantages Quiz
=======================

Early versions of Android didn't have the loader
pattern. It was added to avoid directly querying the
database from the UI code. What are the disadvantages
in directly querying the database from the UI code?
The query could take a long time. It could
be terminated before it completes. Or there are no disadvantages.





38 - Disadvantages Solution
===========================

As I said before, early versions of Android
didn't have the loader model. The original deprecated model
caused frame rate drops whenever many applications had to
re-query their databases. Even simple ones. Databases get far
more complex than what we have here in
Sunshine. So the first answer is a definite yes.
We noted how things like AsyncTask are bound to
the UI, so something as little as an orientation
change could kill the query. So that's another definite
yes. So the third option is just a non-starter.





39 - Using Loaders in our App
=============================

We'll start by adding our call back functions
to our forecast fragment. Now know we have two
different options here for loader call backs. We want to
make sure that we're choosing the android.support.v4 callback so
we're compatible with Gingerbread. These take a generic
type. We're going to want to use cursor. And yes that
is Andriod.database.cursor. Now, let's move down to where we
want to actually insert this code. Here at the bottom
and, of course, we use control I to actually insert.
And there we have it beautifully, our loader pattern with cursor.
Now let's do some work that will help us out. First
we're [INAUDIBLE] of columns to the top of our forecast fragment.
These are the columns that are going to be used in our
query. Note, that since both weather entry and location entry have
the underscore ID field, we must fully qualify, which ID we
want in our projection for this query to work. And in
fact, this query requires a projection to work at
all, otherwise [UNKNOWN] ambiguous. Next, here are indexes that
are tied to these columns. This'll make some of
our work easier, later on. Note, that these actually must
match. Then we create some public indexes to use,
in our adapter. This allows us to make our
code tiny and efficient, in the adapter. But it
means, we must maintain the relationship between these column indexes.
And the projection. Each loader has an ID. It
allows a fragment to have multiple loaders active at
once. We're [INAUDIBLE] place this along with an instance
variable to save off our location here at the
top of our class. Next we're going to override
onActivityCreated. Loaders are initialized
in onActivityCreated because their life
cycle is actually bound to the activity. Not the
fragment. Note the use of the loader ID. FORECAST_LOADER.
Then we return to the onCreateLoader function.
We add a new CursorLoader. This CursorLoader has
our query, including start date, columns, and sort
order. Our start date is actually in our
URI. Our columns are the ones we defined earlier, and our sort order is going
to be based upon COLUMN_DATETEXT ascending. Let's do
one last check. We want to make sure we're
using the support library version of all of
these classes, otherwise we won't get Android 2.3 compatibility.





40 - Moving to Multiple Text Views
==================================

Replace the resource XML for the list item
forecast to split up the single text field
into multiple fields with the IDs below, while
keeping the same approximate design. Note that you
won't just be able to drag additional fields
onto the existing list_item_forecast layout, since it contains
only a text view, which means there is no view group to add additional views to.
I recommend deleting list_item_forecast and creating a new
file if you want to use the visual editor.





41 - Moving to Multiple Text Views
==================================

Alright, you're done. Lets take a look at the solution.
Now we're going to need some text views. First of all lets
look at our linear layout. It's vertical. We're going to want it
to be horizontal. The next thing we're going to want are a
bunch of text views. Lets just drag them in. Now
remember we had all those ID's that were given to us
in the last quiz. The first one is the list item
date text view. And we'll give it some text that makes
sense when we help to layout the layout. We'll remove
this before we actually submit it to our project. And
for fun let's switch to landscape. It'll make things a
little more legible. Also let's just zoom in a little bit.
All right. So that's our list item date text view. Now
right next to that, we want another text view. Since we want the design to be
the same, this one doesn't need to have an ID. If you remember our
design had the date. Followed by a dash, followed
by the forecast, followed by a dash, followed by the high,
followed by a slash, followed by low. So this is just going to be a view that
contains a dash. And we can even hardcode this in. For now, then we're going to
create another text view next to that. Our
placeholder text will be a forecast, like clear.
The ID for this will be list item
forecast text view. Now we need another text view.
Once again this one's not going to have an ID, because we don't need it. And
we'll add another dash. Now we're actually getting
to the high and low forecasts. Let's say
a high of 23. One more plain text view, this one with a slash and, once
again, with no ID. And for our high
forecast, it better be list_item_high_text_view. Finally, we'll add one
more plain text view. This one will be for
our low, and of course, that means it's going to
be list_item_low_text_view. And there we have it. Our multiple text
view layout. Now we might want to make this prettier.
But this is a good start and it just plays
the four fields that we asked for. They are all
part of a linear layout. Before we actually would submit
this, lets remove the placeholder tabs for everything except for,
the static fields. We'll also have to remove these empty IDs, its
actually important to understand whats going on in these text views. And there
we have it, that's what your should look like, At least if you
want to make it look almost identical to the layout we started out
with. Of course you can go wild with these designs, and there's
a lot you can do with the layout editor and with different kinds
of layouts. We'll be learning more about that in the next lesson. Alright,
we finally get to start hooking this up to our UX. Let's go
back to forecast fragment.





42 - Simple CursorAdapter
=========================

Inside a forecast fragment, we're going to replace the mForecastAdaptor
with a simple cursor adaptor that will work with the
new database code we just added. This is another one
we want to make sure we're using the V4 version.
There we are. Make sure to select that in your
list. You can see we have a bunch of code
that doesn't compile. If it's a Ray adapter, let's switch
our Forecast adapter, out with new simple cursor adapter. Now,
simple cursor adapter is pretty cool. It allows us
to map columns within our database, which are in
our weather contract, directly to widgets, that are in
our list items. So, all looks pretty good, except that
it won't quite compile. Because, this stuff we're doing
to start the detail activity isn't going to work.
So for now, let's just put this away. We'll
get back to that later. We have something that compiles.
But we need to do something first. If we
ran it now, we would still get nothing. The reason
why we would still get nothing is we aren't
doing anything in onload finish. In onloader reset we might
as well complete this by doing swapCursor with null.
Our code should actually do something now. Let's try running
it. And it looks terrible. What's in the database doesn't
look anything like what's supposed to be on the screen.
We need some formatting and settings help. More functions for
our utility class. Get preferred location did look a bit
lonely there, right? To help us out, we'll add another
function to utility that tells us if metric is the current
setting. We'll add a simple function to format the temperature,
and we'll add a function to format the date. Now,
interestingly enough, what kind of date is this? Well, we've
got two options, java.util, or java.sql. In this case, we actually
want java.util, but we're missing yet another helper function.
We'll add a function into weatherContract that converts the db
date text into an actual date object, opposite to the
other Contract function. So there we have it. How we
store the data in the database is entirely encapsulated
in the Contract. So, now we've got all that cool
stuff, how do we actually use it? Let's go back
to our forecast fragment. It turns out there's a special
callback for SimpleCursorAdaptor, called ViewBinder.
[BLANK_AUDIO]
Set View Binder, with a single function, Set View Value.
So inside of Set View value, we're actually going to make
really good use of those utility functions we just saw.
First of all, we'll save off as metric. If we're
using the Temperature Columns, then we just conform out the
temperature. Now that we've got setViewValue, let's put some cool
stuff inside of it. You see, that we use isMetric
when we're populating the temperature in our views and we pass
that into our new Utility.formatTemperature function. So you can see, that
having these column indices here allowed us to make this function more
efficient, and it will also help when we move into lesson five.
One of the things you notice, is that the layout height was
really, really small. That's because it was just wrapping content. We
can fix this, by telling it to center our content. And, by
setting the minimum height to the least preferred item height. This will
make things look a little better as well, a lot more like
our original layout. So let's take a quick look to
see what all of this work has done for us.
[BLANK_AUDIO]
That's a little bit more like it. Now as you can see, we're seeing our
forecast in the original way we anticipated. We
can go to our settings and we can
switch things on demand, but now we're in
that same problem I had before. We can't
do anything to get to the detail activity. Well, I'm going to have you fix that.





43 - Fixing our Call to DetailActivity
======================================

Fix the intent call, to lauch our detail activity, by pulling
data from the cursor and using our formatting utility functions to
build our EXTRA_TEXT string. We can use the adapter to get
our cursor. This adapter is passed into our On Item click listener.





44 - Fixing our Call to DetailActivity
======================================

Alright, you're done. Let's start by uncommenting out that text.
As you can see, you need that four cast string
to pass on to our detail activity. So what do
we use to get that four cast string? Well we're lucky
here, because we actually get the adapter view cast in.
All we have to do is cast it at simple
cursor adaptor and then we can get the cursor. We
can move it to our position. Then, we can use string.format
to get a formatted string based on the values
of our columns. Note, once again the nice use
of our column intendacies. One ending curly brace and
let's move that intend stuff into there. And that's it.
Now we'll be able to click on it again
and it'll actually go to our normal detail view.
Let's try it out. So here we are, there's
our list view and we go right into details. Pretty
cool. However, if we change our temperature units to imperial here
doesn't actually update. It would be nice if that, it was
based on the same kind of cursor model we have here.
Something that was observing with the
content observer and would actually update.
[BLANK_AUDIO]
When we changed our setting.





45 - The User Changed their Mind
================================

All right, I get the hint. You actually want
this class to be finished sometime before the next
Android release happens. We're almost done. So we look
in ForecastFragment in onStart. We've actually been cheating a
bit. Every single time we get to onStart, we
actually update the weather. Let's get rid of that.
It'll force us to actually have to use the
refresh function to get new weather, but it'll help illustrate
the cool stuff we're about to do with preferences.
We're going to stop cheating here. In onPreferenceChange, if we're
not actually binding preferences, we're going to execute a new
FetchWeatherTask if there's a change in location. Otherwise, we'll just
notify our CONTENT_URI to allow our cursor to update.
We still have to do one more thing. In ForecastFragment
we've been using an instance variable, mLocation, to save the
preferred location we get when we're actually an onCreate loader.
We've been doing that because we've been planning to
use this for a while. Let's override another function. In
onResume we need to check to see whether or not
our location has changed. So if our location is not
equal to null and Utility.getPreferred location does not equal
location, well, then we can call update weather. Let's see
what happens. You see it immediately changed back. You didn't
see that loading because it didn't have to load anything.
However, if we change settings to something else,
it's refreshing the background. Well, it turns out
updating weather doesn't actually help us here. Why?
Because our URI hasn't changed. Remember, we're doing
everything based upon URI. What we really need
to do is reset our loader. So, how
do we do that? Well, it's pretty straightforward.
Just like we did in onActivityCreated, we have to
use LoaderManager. So instead of calling updateWeather, what
we're actually going to do is call something else.
In this case, not an initLoader, but restartLoader.
It takes the same parameters as an initLoader. Now
let's try this again. That already looks better.
All right, let's change back to our own weather
again. And there we are. As you can see,
the weather is changing as we go from location
to location, but we still have this
problem. Obviously when we change settings here,
it doesn't affect anything, so we need to fix that. That's where you come in.





46 - Implement Details
======================

Now for the big quiz. In all seriousness, this is the most
complicated exercise I've given you so far this lesson. I want you
to implement the details view using the cursor loader and multiple text view
widgets. And I've got some hints for you. Use loaders
not adapters. What does this mean? Well, it makes sense. Your detail
view does not contain a list so you don't
need to use an adapter, but you can still
use the loader pattern. The loader pattern will automatically
observe for changes in the URI with a CursorLoader. The
second thing you want to do is pass the date
into your intent. The date is the important thing to
the detail you need to have passed into it
to properly query the content provider. Now remember, you can
actually get the current location settings from the utility function. So
all you really need is the date. I hope that helps.





47 - Storing Images or Binary Data
==================================

All right. Now for a little fun exercise. If data that we needed to store
included images or other binary data, how would
we store it? Would we put the raw
data in the database? Would we store files
into a public folder on the device? Or
would we store the files in a private
folder, and reference the location in the database?





48 - Storing Images or Binary Data
==================================

Let's talk about this. Honestly, it really depends on
the size of your data. For small amounts of
binary data, very, very small amounts of binary data,
you could actually put the raw data in the
database. There are blob types and those are supported
and work pretty well. However, as you start putting
larger data into the database, it gets pretty inefficient.
And you end up writing a lot more code.
Well, as far as storing the files in a public folder
on the device is concerned, well, that might work if you
actually wanted to share those files. But, typically, you don't want to
share those files with other applications. So, storing them in a
public folder on the device isn't a very good idea. But
the third option is pretty good. Storing the files in a
private folder and then referencing the location in the database. Now,
admittedly, you might not even need to put them into the database
at all. But it's often nice to do this if
you need to do any kind of queries on them. You
can store metadata in the database and store files in
a private folder. This is what Android does in the gallery.





49 - Making your ContentProvider Accessible
===========================================

It's actually quite straightforward to make your apps content provider
available to third party apps. The key is to modify
the export flag in the manifest entry here to true.
As simple as that. Any app that knows the content URI
can use the content resolve or access it the same
that you do. Now depending on the sensitivity of your
data, you may want to protect it by requiring specific
permissions to read or write to the database. So if you
want to, you can effectively limit access only to other
apps you have created, or to third party apps, which know
the permissions and users agree to. Then you just need to
publish this contract, to interact with the content provider, specifically the
URI and column names. And you've created a simple API
for all of your data. And as simple as that, you've
created a new API, just for your data. This is exactly
the same approach, used by many of the native content providers,
including the Contacts Database, Media Store, Calendar and Call Log.





50 - 3rd Party Content Providers
================================

Explore the Native Content Providers linked to in
the instructor notes below. What's the name for the
static constant used to find the content URI
for accessing audio stored on the internal data store?





51 - 3rd Party Content Providers Solution
=========================================

There's a number of different content providers available to
access different kinds of media stored in different locations.
To access audio media stored in the internal data store, you'd use the
INTERNAL_CONTENT_URI provided in the MediaStore.Audio.Media class.





52 - Lesson 4 Recap with Reto and Dan
=====================================

So, Rato, what have we learned in this lesson?
 Well, I learned that I do not want to write
databases and content providers. What did you learn in this lesson?
 Well, J unit tests are awesome, and building
the back end framework of our apps is the
most glorious of work. It's an absolutely vital part
of creating a robust app with a great user experience.
 And I also learned that Dan cannot do an English accent.
 [SOUND]. Fortunately, you can get ready to welcome back Katherine
for lesson 5, where we'll shatter our activity into fragments, use
our content providers to populate more complex UIs, and build layouts
that work as well on tablets as they do on phones.
 Which seems like a convenient segway
into the history and evolution of Android hardware,





53 - Storytime Android Hardware
===============================

Of all the Android launch devices, the Nexus 1 will
always hold a special place in my heart. That's not
to take anything away from my first Android device, the
HTC G1, that launched Android into the world with its
distinctive chin and slide-out keyboard, or the HTC Magic. First
pre release android phone I got to dog food when
I joined google. And the first, the wave of devices
we gave every attendee starting at google IO in 2009.
interesting to global phenomenon. Growing from 100,000 activations a
day in May, to over 300,000 by December.
That was seriously helped by the release of the
Motorola Droid. I can still remember the offices
here in building 44 echoing with the constant refrain
of Droid as each device rebooted. The Droid was
the first of the thinner, higher res, more powerful
Android devices that have since become standard. And it
was also the device whose release on Verizon here in
the U,S. Really kicked off the flood of Android
activations. But I was a Londoner then, and the Droid
was a [UNKNOWN] only phone, so it was never
really a device I truly used. Then, there was the
Nexus One, code-named Passion. The first device I felt was
pure Android was also the phone we brought with us
on our 2010 Android Developer Labs World Tour. Me
and my colleagues travel across the world. Seven cities
in ten days. Introducing over twenty thousand developers to
Android by way of presentations and a free Nexus 1.
As a result, I made a lot of friends
across Europe and learned exactly how many boxes of phones
I can carry across snow drifts in Stockholm and
icy roads in Berlin. Since then, The Nexus S4 and
five have each improved on what it takes, to make
a great android phone. And OEMs across the world, have
created 1000s of different Android devices. Starting with the Motorola
Zoom, which led Android's official expansion into tablets, and leading the
way for the Nexus 7 and 10. But that was
just the start, Those changes paved the way for the
Android mobile [UNKNOWN] west, to grow into what could be,
the brains behind anything, from phones and tablets, to TVs, cars
and wearables, like Android wear. It's impossible to know, what comes
next. Who knows? Maybe the next Android devices, will be Androids themselves.
