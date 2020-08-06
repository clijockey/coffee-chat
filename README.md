# Coffee Chat Demo App [WIP]

This demo makes use of;
* Firebase
* Firestore
* Cloud Storage
* Cloud Functions 

An overview of the flows for the app is;

# Demo


## Pre-Req
* Clone Repo
* Setup Firebase with (follow the CodeLabs at the bottom of page)
    * Cloud Firestore 
    * Cloud Storage
* log into Firebase Project `firebase login`
* Associated cloned repo with firebase `firebase use --add` and correct project ID

## Lets Go

You can run the project locally with `firebase server --only hosting`.

Ensure the Firebase SDK is imported into the app (`index.html`)

```
<script src="/__/firebase/6.4.0/firebase-app.js"></script>
<script src="/__/firebase/6.4.0/firebase-auth.js"></script>
<script src="/__/firebase/6.4.0/firebase-storage.js"></script>
<script src="/__/firebase/6.4.0/firebase-messaging.js"></script>
<script src="/__/firebase/6.4.0/firebase-firestore.js"></script>
<script src="/__/firebase/6.4.0/firebase-performance.js"></script>
```

The following contains the relevant Firebase config;
```
<script src="/__/firebase/init.js"></script>
```

```
if (typeof firebase === 'undefined') throw new Error('hosting/init-error: Firebase SDK not detected. You must include it before /__/firebase/init.js');
firebase.initializeApp({
  "apiKey": "qwertyuiop_asdfghjklzxcvbnm1234568_90",
  "databaseURL": "https://friendlychat-1234.firebaseio.com",
  "storageBucket": "friendlychat-1234.appspot.com",
  "authDomain": "friendlychat-1234.firebaseapp.com",
  "messagingSenderId": "1234567890",
  "projectId": "friendlychat-1234",
  "appId": "1:1234567890:web:123456abcdef"
});
```

### User sign in

Setup to use the Google Auth Provider, you will see functions in `public/scripts/main.js` called `signIn()`, `signOut()` and `initFirebaseAuth()`. Signed in user details are collected with `getProfilePicUrl()` and `isUserSignedIn`

To test you can run `firebase serve --only` on local machine

### Messages
Messages get written (and are retrieved from) Cloud Firestore and can be viewed on the backend via the Firebase console under `Develop->Cloud Firestore`.

### Text is so 1990's, I want images!
If I cant send a picture its not real! The images are saved in Cloud Stroage using the `saveImageMessage()` function.

You can view the backend via the Firebase Console `Develop->Storage`

### Should I not secure this stuff?
You can secure the access to things like Firestore or Cloud Storage via the Firebase console or vai the CLI. You will notice two files with `.rules` extension.

You can define access rights, security and data validations with this.

To deploy the configuration files via the CLI;
```
firebase deploy --only firestore
```
or 
```
firebase deploy --only storage
```

This will look at the `firebase.json` file to undestand where the rules are defined and then apply them.

### Deploy to Firebase Hosting
Running locally is great but at some stage you need to deploy it somewhere.

`firebase deploy --except functions`

### Show Notifications


### Functions
You may need to get the env ready for functions;

```
cd functions
npm install
```
All the functions are defined in `functions/index.js`

To deploy the functions you would issue `firebase deploy --only functions`.

#### welcome New Users
To cuase a push notification you can delete the user logging in via the Firebase console `Develop->Authentication`, now when you log in a message will be pushed welcoming you.

#### Image Modification
What if someone shares an image that you dont want? Making use of Cloud Vision API can help review images and then blur out if not acceptable.

#### New Message Notifications
I would like to get notified when new messages are posted.

More details https://codelabs.developers.google.com/codelabs/firebase-web/index.html?index=..%2F..index#10

### Performance
The following code in `index.html` will allow performamce monitoring;

```
<script type="text/javascript">!function(n,e){var t,o,i,c=[],f={passive:!0,capture:!0},r=new Date,a="pointerup",u="pointercancel";function p(n,c){t||(t=c,o=n,i=new Date,w(e),s())}function s(){o>=0&&o<i-r&&(c.forEach(function(n){n(o,t)}),c=[])}function l(t){if(t.cancelable){var o=(t.timeStamp>1e12?new Date:performance.now())-t.timeStamp;"pointerdown"==t.type?function(t,o){function i(){p(t,o),r()}function c(){r()}function r(){e(a,i,f),e(u,c,f)}n(a,i,f),n(u,c,f)}(o,t):p(o,t)}}function w(n){["click","mousedown","keydown","touchstart","pointerdown"].forEach(function(e){n(e,l,f)})}w(n),self.perfMetrics=self.perfMetrics||{},self.perfMetrics.onFirstInputDelay=function(n){c.push(n),s()}}(addEventListener,removeEventListener);</script>
```

You will also need to have `firebase.performance();` added to `main.js`. You can then browse to `Quality->Performace` in the Firebase console.



# Credits

This app used Google Code Labs to kick start, you can start yourself with code and detailed instructions at [Firebase: Build a Real Time Web Chat App Codelab](https://codelabs.developers.google.com/codelabs/firebase-web/).

