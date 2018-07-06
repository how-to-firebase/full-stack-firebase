# Walkthrough

Fogo is an image-management application, and as such, it needs to handle thousands of images.

We would never attempt to display thousands of images in a browser, so we need to paginate our results.

We've implemented a form of infinite-scroll pagination where a new page is requested whenever the bottom of the list becomes visible to the user.

Firestore collections can be easily paginated with a cursor.

In this case our collection is named "uploads". And we're also filtering by the environment attribute, which is the string "development" in this example.

Filtering on the environment attribute enables us to host multiple versions of this application on a single Firebase project. So in addition to a development environment we have a test environment and a production environment.

Next we define the limitedCollection by adding a .limit filter on the collection.

Finally, if there is an existing cursor then we modify the limitedCollection to start after that cursor.

If there is no cursor, then we don't call the startAfter function and simply request the first 25 records.

We receive the first 25 records and that fills the page just fine.

Now, let's scroll down a bit to trigger another page to load.

This page load will pass in the last image on the page as the cursor.

So now notice that we have a cursor and we're calling limitedCollection.startAfter with cursor.created.

The query that follows results in another 25 records and we've filled another page!

Now to demonstrate using Firestore's onSnapshot function to listen for new records.

After the initial pagination load we will be adding images to the top of the list. These images will have a "created" timestamp before the first record from the pagination.

So we set up a new query for the uploads collection. Again, we filter on the environment.

This time we also put on a second "where" clause to require the "created" attribute to be after the lastCreated timestamp.

Now we'll use the onSnapshot function to listen for new records.

The initial snapshot result is null, because there are no records uploaded after our lastCreated timestamp.

However, as soon as a new record hits the database with a more recent timestamp, the onSnapshot listener fires with all of the results.

Note that the snapshot will have all of the results that match the criteria.

We've just uploaded one file in this case, but if we were to upload a second file, the onSnapshot listener would fire again, but this time with two results.

onSnapshot fires whenever the query results change, and it returns all of the query results every time.

And there it is! We have infinite-scroll pagination as well as a listener that will tack new records onto the top of our list as they arrive!

