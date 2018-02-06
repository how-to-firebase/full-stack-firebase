Welcome to Full‐Stack Firebase with Juarez Filho and me, Chris Esplin.
Juarez and I love web development, and we particularly love developing web applications
on the Firebase platform.

Firebase is an integrated application‐development platform. Think of it as a suite of
services that all work together so that you can write less code while still developing robust,
scalable applications.

# 1
The starting point for any Firebase project is the Firebase Dashboard, and that’s where we
will start right now.

So fire up a web browser to follow along, or sit back and absorb for the next few minutes
while we get started with Full‐Stack Firebase

# 2
You'll start off your Firebase journey on console.firebase.google.com. You'll need to
be logged into your favorite Google account for this, so stop what you're doing and
make sure that you have a Google account. A Gmail account ***is*** a Google
account, so you likely already have one.
Now log into your Google account and navigate to
[console.firebase.google.com](https://console.firebase.google.com/).
If you have existing Firebase projects you’ll see them here.
Each Firebase project is also a Google Cloud Platform (also known as a GCP) project.
When you create a project on your Firebase console you will automatically create a
new project on the Google Cloud Platform console. We’ll cover this briefly later. Just
know that Firebase is really Google Cloud Platform. It just has its own web console,
and that’s a great thing, because the Google Cloud Platform is a sprawling system,
and it’s great to have a more Firebase‐focused view.
I have a ton of existing projects, but your account is likely empty, so click “Add
project” to begin your first Firebase project!

# 3
Now it’s time to make some decisions.
Like I said earlier, creating a Firebase project will create a GCP project as well, so your
project ID must be unique across all of Google Cloud Platform.
Additionally, a single Firebase project can host web, Android and iOS.
The associated GCP project can host servers, DNS… basically everything that you could
conceivably need for any application.
But let’s ignore the rest of Google Cloud Platform for now and focus on Firebase! Pick a
name and a country/region—preferably close to you or your customers—and click Create
Project.

# 4
And now we have a fresh, new Firebase project!
We’re on the Project Overview page. Notice the prompts to add Firebase to your iOS app,
Android app or your web app.

# 5
We’ll stick with web.
Click to open up some instructions.

# 6
The “add Firebase to your web app” modal includes two script tags that you can copy/paste
directly into your app.
We’ll use these script tags later. It’s just nice to know that they’re here.
And maybe notice that there are two steps to including Firebase in your web app.
The first script tag adds the SDK to your page.
The second script tag uses some project‐specific details to initialize Firebase to your unique
project.
These API keys are all public, so don’t worry about publishing them. Every user who hits
your app will need these keys to interact with your Firebase project.

# 7
Now close the modal and click the “Go to docs” link in the top‐right corner of the page.
This will open a new tab with the Firebase docs in it.

# 8
The Firebase docs are the key to becoming an amazing Firebase developer.
We can do exercises for each feature and teach the basics, but you’ll eventually need the
docs to continue learning.
Notice the five tab links,
‐ Overview
‐ Guides
‐ Reference
‐ Samples
‐ and Libraries.

# 9
We spend most of our time in the Guides. It’s where the Firebase team publishes code
snippets for each feature and gives you suggestions on how to use them.
We keep the Guides page pinned to a browser tab right alongside our Firebase Dashboard
tab during our Firebase development process.

# 10
Also make note of the third tab link: Reference
The reference tab contains full‐blown docs for each and every object and function in the
Firebase SDK.
It can be a lot to digest, so we don’t use it often… however, when you need to know exactly
what arguments a function can accept, the Reference tab is a great friend.

# 11
If you’d like to click around…
The Get started for Web section covers browser‐specific code
The Get started for Admin section covers the Node.js SDK that you’ll use with Cloud
Functions later on to securely administer your Firebase project.
Feel free to take a five‐minute break to noodle around the docs a bit. Go ahead and pause
here and come back when you’re ready to continue our Dashboard overview.

# 12
…
And welcome back!
Navigate back to your Dashboard and let’s click the “Alerts” bell in the top‐right corner next
to your user Icon.

# 13
And you don’t have any alerts 
You may get some eventually, And if you do, you’ll access them here!
You can click the “gears” settings button if you’d like to configure your alerts… but let’s skip
that for now.
Moving on!
Close the Alerts drawer and click the “gears” icon on the left side next to Project Overview.

# 14
You’ll see two links pop up.
The first is your Project Settings. That’s where we’ll head next.
Below that is your Users and permissions link. It will open up your Google Cloud Platform
console in a new tab and enable you to add other Google accounts to your project as well
as manage your secure API keys.
But let’s start with Project Settings.

# 15
Your project settings has tabs for
‐ General,
‐ Cloud Messaging,
‐ Analytics,
‐ Account Linking,
‐ and Service Accounts.
We’ll use the Service Accounts tab in the Cloud Functions module, but let’s not get
distracted.
I don’t recommend editing your project names. It can get confusing. So leave everything
here as you found it and click the Authentication tab on the left…

# 16
Here we go.
Authentication is under the Develop banner. Notice there are also sections for Stability,
Analytics and Grow.
And below that is an Upgrade button to migrate to a paid plan.
Stability, Analytics and Grow are only for iOS and Android apps. We don’t get to use them
for web apps, and that’s just fine. We’ll have plenty to keep us busy.
So… authentication.
The Authentication tab links are
‐ Users,
‐ Sign‐in Method,
‐ Templates, and
‐ Usage.
Let’s start on the Users tab. It’s got a list of authenticated users. We can add and delete
users here. We can also send password reset emails.
Now let’s click on the Sign‐in Method tab.

# 17
We usually start by enabling email/password authentication and Google Authentication.
You can enable the other auth methods as needed.
We’ll cover this is much more depth later.
…next, head to the Templates tab.

# 18
Firebase Authentication can send some email for you.
It’s all pretty basic and not worth messing with much until you’re headed to production.
Just be aware that you can customize your Authentication emails here.
We’ll skip the Usage tab for now.
Now on to the Database page!
Each page is accessible from the left accordion menu. So click on the Database link under
the Develop header to follow along.

# 19
The Database page gives you two options,
‐ Realtime Database and
‐ Cloud Firestore
The Realtime Database is where Firebase got started.
Cloud Firestore is where Firebase is headed.
Don’t get us wrong. The Realtime Database is still a first‐class piece of the Firebase
platform. It’s still getting new features and is running most Firebase apps.
However, Cloud Firestore is where you should be focusing your development and learning
efforts. It’s still in beta as of early 2018, but it’s one of Google’s extended betas. You can
start using it now, and you’ll only benefit as the product matures.
But let’s start with the Realtime Database. It’s where Firebase began, and you need to
understand it to get why Firebase is such a big deal.

# 20
The Realtime Database is a single JSON object.
That’s JSON as in JavaScript Object Notation.
As a simple JSON object, you can interact with it natively in JavaScript. It’s also entirely
unstructured.
You have to impart your own structure to the Realtime Database.
It’s a blessing and a curse. If you do it right, the Realtime DB is the fastest, most flexible
development database you’ll ever use.
If you approach the Realtime DB too naively and make one of many common mistakes…
well… start rewriting your app now 
Fortunately, we are well aware of the Realtime DB’s pitfalls and will steer clear of them all.
Now, let’s click through this page’s tabs...

# 21
First is Security Rules for the Realtime DB.
The Realtime DB is public‐facing. Your browser connects to it directly, not through a server
API.
A public‐facing database requires data‐level security, and this is what you’ll get with
Security Rules.
This is a whole topic, and we’ll cover it in depth later on.
… so moving on…

# 22
Backups require a paid plan, but that’s all the work you’ll need to put forth.
Upgrade to Blaze, which is Firebase’s production, pay‐as‐you‐go plan, and visit the Backups
tab to enable Realtime Database backups.
You’ll definitely want backups for your production application. The great part is that it’s
cheap. You’re merely paying for the storage space on Google Cloud Storage, which is quite
economical.

# 23
Now notice the Database dropdown.
This is a shortcut to switch between the Realtime Database and Cloud Firestore.
24
Let’s jump across to Cloud Firestore to see a much more structured way to manage data.

# 25
And this is the Cloud Firestore dashboard.
It’s a little different than the Realtime Database, because it’s a document/collection
database instead of a JSON database.
You may have heard of other document/collection databases such as MongoDB or
CouchDB.
Well, Firestore is document/collection for the cloud. Your users will access it directly from
their web browsers, just like they do with the Realtime Database.

# 26
Of course, you’ll need security rules for Cloud Firestore.
The Cloud Firestore security rules look a little different from the Realtime Database’s
security rules, because the Firebase team is moving toward a new security rules
specification.
It turns out that the security rules system for the Realtime Database does not work well for
other Firebase services, so all subsequently‐developed Firebase services use this newer
style of security rules.

# 27
We’ll look at the Indexes tab later. Cloud Firestore is much more structured than the
Realtime Database, and that structure enables much more complex queries.
But to keep complex queries performant, you’ll need to specify custom indexes. Again,
we’ll hit this later in depth.
Usage is self‐explanatory. Just remember that it exists and you’ll know where to find it
when you need it.
Now on to Storage!

# 28
Firebase Storage is for all of those images and PDFs and BLOBs that you don’t want to stuff
into the Realtime Database or Cloud Firestore.
Storage is backed by Google Cloud Storage, which is roughly equivalent to Amazon S3.
You pay for a Google Cloud Storage bucket, and that’s about it. Your objects land in the
bucket and can be manipulated with the Cloud Storage SDK, which is not a Firebase
product.
But let’s not undersell Firebase Storage. They’ve solved a nasty little problem here.
The old server‐centric model of uploading files involved encoding a file in the browser,
streaming it to a server that you managed, and then streaming it from your server up to
your storage provider.
Managing these data streams was always a huge pain in the neck. It’s a brittle process that
requires advanced coding chops.
Firebase Storage handles all of that for you, enabling your users to upload files directly
from their browsers to your Cloud Storage bucket.
That’s it. It’s simple from the developer’s perspective. And if you’ve never streamed files to
your own server… well… you may never have to face that pain.
Firebase Storage is one of those slam‐dunk features that we’d use even if we weren’t using
the Firebase databases. It’s just too useful to pass up.

# 29
And here, for the third time, we see security rules.
Storage security rules look like Cloud Firestore security rules. They use the new syntax.
Firebase Storage is, again, a public‐facing service. You need to set security rules or any
lousy hacker will mercilessly take over your Cloud Storage bucket.
Fortunately, Firebase security rules are pretty simple. Yeah, they may look ugly, but it’s
unavoidable ugliness. It doesn’t get simpler than this, and you’ll understand it all soon
enough.
Next up is Firebase Hosting. Click the Hosting link on the left bar to follow along.

# 30
Firebase Hosting is for your static assets. Usually these static assets are your web
application files, so mostly HTML, JavaScript, CSS and whatever icons or images you use in
your web app.
No code is executed here. This isn’t a Node.js or a PHP server, so you can’t upload a .php
file and expect it to get executed.
Firebase Hosting does not support server‐side applications like Wordpress. You’ll need a
full‐blown Apache or Nginx web server for that.
The serverless model that we use with Firebase involves a static web application, so all of
these files are meant to be executed in a web browser.
Now you can connect a custom domain using this tab, so instead of quiverfour.
firebaseapp.com, you’ll be able to access this web app at fogo.howtofirebase.com and
fogo.calligraphy.org.

# 31
Scrolling down, you’ll see your deployment history.
I deployed quite a few times as I developed this web app, and I can quickly revert to any of
my old deploys by clicking one of those arrow buttons on the left.
That’s it for hosting. It’s a powerful feature, but you’ll interact with it mostly via your
terminal and the Firebase Tools command‐line interface.
So let’s transition over to Cloud Functions for Firebase.

# 32
You could lose yourself for months in the depths of Cloud Functions.
We’ll cover it at a high level. Just know that there is a lot more where this comes from.
Cloud Functions is a total game changer. It let’s you go truly serverless, worrying not at all
about your infrastructure and relying on Google and the Firebase team to scale your apps
on the fly.
This is a mind‐blowing way to develop. If a function has zero calls, you don’t pay a dime. If
it get one millions calls, then Firebase executes your function one million times and you pay
for those one million calls, and not a single call more.
Our development apps have Cloud Functions bills in the cents. Like less than one dollar a
month. Hundredths of a dollar.
And all we have to do to scale from one user to a huge app is to keep paying the bill. It
doesn’t get easier than that.

# 33
The big, fat downside of developing in Cloud Functions is that you don’t have any access to
the infrastructure that’s running the functions.
This is a common problem across functions‐as‐a‐service providers.
And that’s why you need to make friends with your Cloud Functions logs. You’ll log out
errors and test data from your Cloud Functions and you can view those logs here.
We’ll develop our Cloud Functions with a test‐driven‐development or TDD development
cycle. This will enable us to develop locally where we have a quick feedback loop.
We’ll then export those functions to Cloud Functions for Firebase and do some manual
testing to validate that they’re behaving correctly.
The Cloud Functions logs help a lot with this process.

# 34
Now we all know that cloud services come with a bill, so let’s see what we’re up against by
clicking on the “Upgrade” button and popping up the pricing modal.

# 35
… and here we are.
Firebase pricing is broken up into three tiers,
‐ Spark,
‐ Flame, and
‐ Blaze.
Spark is the free tier. It’s quite generous, but it has some restrictions, and we don’t like
restrictions at all. Restrictions are garbage, and we’re willing to pay to remove them.
Flame is the fixed‐price tier. It’s $25/month, and we can’t recommend it. Yes, it’s a fixed
price, and some folks need predictability. But it’s never going to be cheaper than Blaze. The
only advantage is that you can’t accidentally blow through your $25/month price cap.
Blaze is what we use for all of our apps, even in development mode. We highly recommend
you upgrade to Blaze if you can.
Blaze is variable pricing. So most of our development apps cost either nothing or just a few
dollars a month. It’s crazy cheap for development, and the price is 100% variable as you
scale to a huge app.
Have you ever paid $7/month for Digital Ocean or Heroku? Well, Blaze is usually much
cheaper than that. Google and the Firebase team have taken the approach of keeping
Firebase’s pricing accessible in order to build market share.
Don’t worry. They will make plenty of cash off of you once your apps reach a large
audience.

# 36
And notice how Blaze has zero limitations. This is the key. You get full use of all of Firebase’s
un‐metered services and an unfettered Google Cloud Platform project for any extended
services that you need.
We’d like to quickly address some rumors that pop up every once in a while regarding
Firebase’s billing practices.
Yes, it is possible to architect your application horribly and run up a massive bill. This is
possible with any cloud service. It’s just part of the territory.
We’ll give you all of the tools to build price‐efficient Firebase apps. The biggest
considerations are around how each service gets billed out.
For instance, Cloud Firestore charges more for read/writes than for bandwidth. So we’ll be
careful with how many reads and writes we make to Firestore, but we won’t worry much
about network bandwidth.

# 37
The Realtime Database, on the other hand, charges based on network traffic and the size of
your database, not based on read/write count. So we’ll use the Realtime DB for features
that have high read/write volume but low bandwidth requirements. This make the
Realtime Database great for chatty protocols, like internet‐of‐things applications… but you
wouldn’t use it for storing massive object graphs.
It’s not rocket science. Anyone can read the pricing guides and see how each feature gets
billed out. You’ve been warned. Firebase is quite price‐competitive, but if you spam the
service, don’t be surprised when you get the bill.

# 38
Now let’s run through the other left‐side banners.
You won’t need these… but you’ll be seeing them, so let’s not be strangers.
The Stability accordion menu has Crashlytics, Crash Reporting, Performance and Test Lab.
These are killer features for your native Android or iOS apps… but not so useful for web.

# 39
Analytics has a whole bunch of features for Android and iOS.
We don’t use these, because we write for the web. We use Google Analytics, which is a
separate Google product made for the web.
But those poor Android and iOS devs were never first‐class citizens in Google Analytics, so
Firebase gave them their own Analytics services… which we don’t’ get to use.

# 40
And finally, Grow.
None of these are applicable for web. Again.
But don’t worry. Google has no shortage of tools for web… they’re just not part of Firebase.
Google’s web tools are not specific to Firebase and don’t belong in the product anyway.
If you’re feeling jealous of Firebase’s AdMob integrations, try Google’s display and search
ad networks. They’re web‐first and have been around for over a decade.

# 41
Thanks for watching!
@JuarezPAF
@ChrisEsplin
HowToFirebase.com
github.com/how-to-firebase
… and that’s the Firebase Dashboard! As usual, we’ve just scratched the surface.
Follow along for deeper dives into Firebase’s feature set.
We’re Juarez Filho and Chris Esplin, and you can find us and our Firebase web apps all over
the internets.
Tweet at us on the Twitter and check out HowToFirebase.com and our GitHub for more
Firebase‐related codes.
Thanks for watching Full‐Stack Firebase, and happy programming!
