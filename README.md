# Cordova-Hello-World
Steps for creating a simple Cordova project.

Install android and iOS sdk's.

Install nodejs and git.

Navigate to a directory where you would like to create your Cordova project.

To bootstrap an application, type in the following command:

**cordova create toptal toptal.hello HelloToptal**




*Project Structure:*

toptal/


|-- hooks/


|-- platforms/


|-- plugins/


|-- www/


`— config.xml




The www folder contains your application core. This is where you will place your application code which is common for all platforms. (HTML, CSS, and JS).


config.xml file is where you can change metadata for your application, such as author and description.




*Next steps:*


**cordova platform add android**


**cordova platform add ios**


**cordova build** (Builds both platforms)


or 


**cordova build ios** / **cordova build android**


**cordova run ios** or **cordova run android**


By defualt the app will run on your device if it's connected. Otherwise it will run on the platfroms emulator.




*Changing app icon.*


Go to the config.xml file.


**icon src="res/icon.png"**


To use a different images for different platforms use:


**icon src="res/ios/icon.png" platform=“platform" width="57" height="57" density="mdpi"**


<https://cordova.apache.org/docs/en/latest/config_ref/images.html>



Adding a splash screen requires the following plugin:


**cordova plugin add cordova-plugin-splashscreen**


The splash resource goes in the config.xml file.


<https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-splashscreen/>
