#Cordova installation and plugins for native features.

Here you will find instructions for installing Cordova and creating your first project. You will also find instructions and code samples for implementing native android and ios features through plugins in your Cordova project.

## 0. Index
1. [FirstSteps](#1-firststeps)
2. [Icon](#2-icon)
3. [SplashScreen](#3-splashscreen)
4. [Contacts](#4-contacts)
5. [TouchID](#5-touchid)
6. [3Dtouch](#6-3dtouch)
7. [Share](#7-share)
8. [DeepLinking](#8-deeplinking)
9. [AutoFocus](#9-autofocus)



## 1. FirstSteps


1. Install android and iOS sdk's.
2. Install nodejs and git.
3. ```npm install -g cordova```
4. Navigate to a directory where you would like to create your Cordova project.
5. ```cordova create "MyApp"```


*Project Structure:*

MyApp/


|-- hooks/


|-- platforms/


|-- plugins/


|-- www/


`— config.xml



The www folder contains your application core. This is where you will place your application code which is common for all platforms. (HTML, CSS, and JS).


config.xml file is where you can change metadata for your application, such as author and description.




*Next steps:*


```cordova platform add android```


```cordova platform add ios```


```cordova build``` (Builds both platforms)


or 


```cordova build ios```/ ```cordova build android```


```cordova run ios```or ```cordova run android```


By defualt the app will run on your device if it's connected. Otherwise it will run on the platfroms emulator.







## 2. Icon



Go to the config.xml file.


**icon src="res/icon.png"**


To use a different images for different platforms use:


**icon src="res/ios/icon.png" platform=“platform" width="57" height="57" density="mdpi"**


<https://cordova.apache.org/docs/en/latest/config_ref/images.html>







## 3. SplashScreen


Adding a splash screen requires the following plugin:

**cordova plugin add cordova-plugin-splashscreen**


The splash resource goes in the config.xml file.


<https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-splashscreen/>







## 4. Contacts




In order to access user contacts you must first install the following plugin:

```cordova plugin add cordova-plugin-contacts```

*This plugin defines a global navigator.contacts object, which provides access to the device contacts database.
  Although the object is attached to the global scoped navigator, it is not available until after the deviceready event.

**Methods:**


*navigator.contacts.create*

*navigator.contacts.find*

*navigator.contacts.pickContact*




**navigator.contacts.create:**

The navigator.contacts.create method is synchronous, and returns a new Contact object.

Example:   
  
  var contact = navigator.contacts.create();
  
  contact.name = "Raynaldie Acevedo"
  
  contact.displayName = "Ray";  *ios
  
  contact.nickname = "Ray"; *android 
  
  contact.phoneNumbers = [array of phone numbers];
  
  Ex.
  
  var phoneNumbers = [];
  
  phoneNumbers[0] = new ContactField('work', '768-555-1234', false);
  
  phoneNumbers[1] = new ContactField('mobile', '999-555-5432', true); // preferred number
  
  phoneNumbers[2] = new ContactField('home', '203-555-7890', false);
  
  contact.emails = [array of email addresses];
  
  
There are plenty more feilds. For a complete list check the docs.

<https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-contacts/>
  
  
Once you have all the feilds you want and like the contact saved to the device invoke the follwing:

  contact.save(success callback, error callback);
  
  
  

**navigator.contacts.find**

The navigator.contacts.find method executes asynchronously, querying the device contacts database and returning an array of 
Contact objects.

The resulting objects are passed to the contactSuccess callback function specified by the contactSuccess parameter.

The contactFields parameter specifies the fields to be used as a search qualifier.

A zero-length contactFields parameter is invalid and results in ContactError.INVALID_ARGUMENT_ERROR.

A contactFields value of "*" searches all contact fields.

The contactFindOptions.filter string can be used as a search filter when querying the contacts database.

If provided, a case-insensitive, partial value match is applied to each field specified in the contactFields parameter.

If there's a match for any of the specified fields, the contact is returned. Use contactFindOptions.desiredFields parameter 
to control which contact properties must be returned back.

Supported values for both contactFields and contactFindOptions.desiredFields parameters are enumerated in ContactFieldType 
object.

Parameters

contactFields: Contact fields to use as a search qualifier. (DOMString[]) [Required]

contactSuccess: Success callback function invoked with the array of Contact objects returned from the database. [Required]

contactError: Error callback function, invoked when an error occurs. [Optional]

contactFindOptions: Search options to filter navigator.contacts. [Optional]

  Keys include:

  filter: The search string used to find navigator.contacts. (DOMString) (Default: "")

  multiple: Determines if the find operation returns multiple navigator.contacts. (Boolean) (Default: false)

  desiredFields: Contact fields to be returned back. If specified, the resulting Contact object only features values for 
  these fields. (DOMString[]) [Optional]
  
  hasPhoneNumber(Android only): Filters the search to only return contacts with a phone number informed. (Boolean) (Default: false)
  

Example 1:
```
   function onDeviceReady() {

   navigator.contacts.find(["*"], onSuccess, onError);

   }
   
   ```

```
  function onSuccess(contacts) {

    document.getElementById("foo2").innerHTML = 'Found ' + contacts.length + ' contacts.';

   }
```

   ```
   function onError(contactError) {

   console.log("Error");

    alert('Error!');

   }

```

Example 2:

```

   function onSuccess(contacts) {

    alert('Found ' + contacts.length + ' contacts.');

   };
   
   ```

```

   function onError(contactError) {

    alert('onError!');

   };
   
   ```


// find all contacts with 'Bob' in any name field

var options      = new ContactFindOptions();

options.filter   = "Bob";

options.multiple = true;

options.desiredFields = [navigator.contacts.fieldType.id];

options.hasPhoneNumber = true;

var fields       = [navigator.contacts.fieldType.displayName, navigator.contacts.fieldType.name];

navigator.contacts.find(fields, onSuccess, onError, options);


For further details check the docs.

<https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-contacts/>



**navigator.contacts.pickContact**

The navigator.contacts.pickContact method launches the Contact Picker to select a single contact. The resulting object is passed to the contactSuccess callback function specified by the contactSuccess parameter.

Parameters
contactSuccess: Success callback function invoked with the single Contact object. [Required]
contactError: Error callback function, invoked when an error occurs. [Optional]

Example:


  ``` 
  function opencontacts() {
  
    navigator.contacts.pickContact(function(contact){
  
    console.log("pickContact");
  
    document.getElementById("foo3").innerHTML = 'The following contact has been selected:  ' + contact.name.formatted;
  
    },function(err){
  
        console.log('Error: ' + err);
  
    });
   }
```



## 5. TouchID



Minimum iOS version is 8 (error callbacks will be gracefully invoked on lower versions).

Requires a fingerprint scanner, so an iPhone 5S or newer is required.

In order to enable TouchId within your Cordova project you must install the following plugin:

```cordova plugin add cordova-plugin-touch-id```


First you'll want to check whether or not the user has a configured fingerprint scanner. You can use this to show a 'log in 
with your fingerprint' button next to a username/password login form.

```
window.plugins.touchid.isAvailable(
  
  function(msg) {alert('ok: ' + msg)},    // success handler: TouchID available
  
  function(msg) {alert('not ok: ' + msg)} // error handler: no TouchID available

);
```
If the onSuccess handler was called, you can scan the fingerprint. There are two options: verifyFingerprint and 
verifyFingerprintWithCustomPasswordFallback. The first method will offer a fallback option called 'enter passcode' which 
shows the default passcode UI when pressed. The second method will offer a fallback option called 'enter password' (not 
passcode) which allows you to provide your own password dialog.
```
window.plugins.touchid.verifyFingerprint(

  'Scan your fingerprint please', // this will be shown in the native scanner popup

   function(msg) {alert('ok: ' + msg)}, // success handler: fingerprint accepted

   function(msg) {alert('not ok: ' + JSON.stringify(msg))} // error handler with errorcode and localised reason

);
```
The errorhandler of the method above can receive an error code of -2 which means the user pressed the 'enter password' 
fallback.

```
window.plugins.touchid.verifyFingerprintWithCustomPasswordFallback(

  'Scan your fingerprint please', // this will be shown in the native scanner popup

   function(msg) {alert('ok: ' + msg)}, // success handler: fingerprint accepted

   function(msg) {alert('not ok: ' + JSON.stringify(msg))} // error handler with errorcode and localised reason

);
```
This will render a button labelled 'Enter password' in case the fingerprint is not recognized. If you want to provide your 
own label ('Enter PIN' perhaps), you can use awkwardly named function (added in version 3.1.0):

```
window.plugins.touchid.verifyFingerprintWithCustomPasswordFallbackAndEnterPasswordLabel(

  'Scan your fingerprint please', // this will be shown in the native scanner popup

  'Enter PIN', // this will become the 'Enter password' button label

   function(msg) {alert('ok: ' + msg)}, // success handler: fingerprint accepted

   function(msg) {alert('not ok: ' + JSON.stringify(msg))} // error handler with errorcode and localised reason

);
```
You can copy-paste these lines of code for a quick test:


```
<button onclick="window.plugins.touchid.isAvailable(function(msg) {alert('ok: ' + msg)}, function(msg) {alert('not ok: ' + msg)})">Touch ID available?</button>
<button onclick="window.plugins.touchid.verifyFingerprint('Scan your fingerprint please', function(msg) {alert('ok: ' + msg)}, function(msg) {alert('not ok: ' + JSON.stringify(msg))})">Scan fingerprint</button>
```

<https://github.com/Telerik-Verified-Plugins/TouchID>



## 6. 3Dtouch


Only avaiable on the iPhone6s 3Dtouch also Quick Action for Home Screen icon (Static and Dynamic) and external links preview.

Required plugin: ```cordova plugin add cordova-plugin-3dtouch```


**Quick action for home screen icon**

```
  ThreeDeeTouch.configureQuickActions([
    {
      type: checkin, // optional, but can be used in the onHomeIconPressed callback
      title: 'Check in', // Mandatory. Action name.
      subtitle: 'Quickly check in', // Short Description of action.
      iconType: 'Compose' // optional
    },
    {
      type: 'share',
      title: 'Share',
      subtitle: 'Share like you care',
      iconType: 'Share'
    },
    {
      type: 'search',
      title: 'Search',
      iconType: 'Search'
    },
    {
      title: 'Show favorites',
      iconTemplate: 'HeartTemplate' // from Assets catalog
    }
  ]);
  ```
  
  iconType is for when you want to use stock ios images like Compose, Play, Pause, Add, Location, Search, Share, Prohibit, Contact, Home, MarkLocation, Favorite, Love, Cloud, Invitation, Confirmation, Mail, Message, Date, Time, CapturePhoto, CaptureVideo, Task, TaskCompleted, Alarm, Bookmark, Shuffle, Audio, and Update.
  
  iconTemplate is used for custom images. It must be a valid name of an icon template in your Assets catalog.
  
  When a home icon is pressed, your app launches and this JS callback is invoked. I found it worked reliable when you use it like this (you should recognize the type params used previously):
  
  ```
  document.addEventListener('deviceready', function () {
    ThreeDeeTouch.onHomeIconPressed = function (payload) {
      console.log("Icon pressed. Type: " + payload.type + ". Title: " + payload.title + ".");
      if (payload.type == 'checkin') {
        document.location = 'checkin.html';
      } else if (payload.type == 'share') {
        document.location = 'share.html';
      } else {
        // hook up any other icons you may have and do something awesome (e.g. launch the Camera UI, then share the image to Twitter)
        console.log(JSON.stringify(payload));
      }
    }
  }, false);
  ```
  
  
  Enable link previw:
  
  ```
  ThreeDeeTouch.enableLinkPreview();
  ```
  
Yeap that simple.

More info: <https://github.com/EddyVerbruggen/cordova-plugin-3dtouch>


## 7. Share

In order to use the native share feature you must first install the plugin below

```
cordova plugin add cordova-plugin-x-socialsharing
```
 
Followed by 
```
cordova prepare
```

The main method of the plugin is

```
window.plugins.socialsharing.shareWithOptions(options, onSuccess, onError);
```

This is the complete list of currently supported params you can pass to the plugin (all optional):

```
var options = {
  message: 'share this', // not supported on some apps (Facebook, Instagram)
  subject: 'the subject', // fi. for email
  files: ['', ''], // an array of filenames either locally or remotely
  url: 'https://www.website.com/foo/#bar?a=b',
  chooserTitle: 'Pick an app' // Android only, you can override the default share sheet title
}

var onSuccess = function(result) {
  console.log("Share completed? " + result.completed); // On Android apps mostly return false even while it's true
  console.log("Shared to app: " + result.app); // On Android result.app is currently empty. On iOS it's empty when sharing is cancelled (result.completed=false)
}

var onError = function(msg) {
  console.log("Sharing failed with message: " + msg);
}

```

More methods can be found in the docs along with quirks and code samples.

<https://github.com/EddyVerbruggen/SocialSharing-PhoneGap-Plugin>

  
## 8. DeepLinking

In order to use deepLinking install the following plugin:

```
cordova plugin add cordova-plugin-customurlscheme --variable URL_SCHEME=mycoolapp
```
Now, if installed on the users phone, your app can luanch with any of the following links.

mycoolapp://

mycoolapp://somepath

mycoolapp://somepath?foo=bar

mycoolapp://?foo=bar

mycoolapp://red

mycoolapp://blue

Note: "mycoolapp" is the value of URL_SCHEME you used while installing this plugin.

When your app is launched by a URL, you probably want to do something based on the path and parameters in the URL. For that, you could implement the (optional) handleOpenURL(url) method, which receives the URL that was used to launch your app.

```
function handleOpenURL(url) {
  console.log("received url: " + url);
}
```

EX.

```
<!DOCTYPE html>
<html>
    <body>
      <br></br>
      <br></br>
      <br></br>
      <p>openwithurl2</p>
      <p id="demo"></p>
      <br></br>
      <script type="text/javascript" src="cordova.js"></script>
      <script type="text/javascript" src="js/index.js"></script>
    </body>
</html>
```

```
var app = {
    // Application Constructor
    initialize: function() {
        this.bindEvents();
    },
    // Bind Event Listeners
    // Bind any events that are required on startup. Common events are:
    // 'load', 'deviceready', 'offline', and 'online'.
    bindEvents: function() {
        document.addEventListener('deviceready', this.onDeviceReady, false);
        function handleOpenURL(url) {
        console.log("received url: " + url);
        if (url === "openwithurl2://red"){
          document.getElementById("demo").innerHTML = "Red Red Red Red Red Red Red Red Red Red Red Red Red Red Red";
        }
        if (url === "openwithurl2://blue"){
          document.getElementById("demo").innerHTML = "Blue Blue Blue Blue Blue Blue Blue Blue Blue Blue Blue Blue Blue ";
        }
    },
    // deviceready Event Handler
    //
    // The scope of 'this' is the event. In order to call the 'receivedEvent'
    // function, we must explicitly call 'app.receivedEvent(...);'
    onDeviceReady: function() {
        app.receivedEvent('deviceready');
    },
    // Update DOM on a Received Event
    receivedEvent: function(id) {
        var parentElement = document.getElementById(id);
        var listeningElement = parentElement.querySelector('.listening');
        var receivedElement = parentElement.querySelector('.received');

        listeningElement.setAttribute('style', 'display:none;');
        receivedElement.setAttribute('style', 'display:block;');

        console.log('Received Event: ' + id);
    }
};

app.initialize();
```

More Info:<https://github.com/EddyVerbruggen/Custom-URL-scheme>


## 9. AutoFocus 

AutoFocus does not require a plugin.

```
<!DOCTYPE html>
<html>
    <body>
      <br></br>
      <br></br>
      <br></br>
      <input type="text" id="firstName" placeholder="First Name" autofocus>
      <br></br>
      <input type="button" onclick=window.open("lastname.html"); />
      <script type="text/javascript" src="cordova.js"></script>
      <script type="text/javascript" src="js/index.js"></script>
      <script>
          document.getElementById("firstName").focus();
      </script>
    </body>
</html>
```
And add the follwing code to your config.xml
```
<preference name="KeyboardDisplayRequiresUserAction" value="false" />
```
