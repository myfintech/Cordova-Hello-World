1. [Steps for creating a Cordova project.](#1-stepsforcreatingacordovaproject)
2. [Changing app icon.](#2-changingappicon)



________________________________________
# 1. **Steps for creating a Cordova project.**
________________________________________

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






_____________________
# 2. **Changing app icon.**
_____________________


Go to the config.xml file.


**icon src="res/icon.png"**


To use a different images for different platforms use:


**icon src="res/ios/icon.png" platform=“platform" width="57" height="57" density="mdpi"**


<https://cordova.apache.org/docs/en/latest/config_ref/images.html>






_____________________________________________________
**Splash Screen.**
_____________________________________________________

Adding a splash screen requires the following plugin:

**cordova plugin add cordova-plugin-splashscreen**


The splash resource goes in the config.xml file.


<https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-splashscreen/>






__________________________________________________________
**Accessing Device Contacts.**
__________________________________________________________



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



__________________________________________________________________________________________________________
**TouchID (iOS).**
__________________________________________________________________________________________________________



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



