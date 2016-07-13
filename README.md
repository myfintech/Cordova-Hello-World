________________________________________
*Steps for creating a Cordova project.*
________________________________________

1. Install android and iOS sdk's.
2. Install nodejs and git.
3. **npm install -g cordova**
4. Navigate to a directory where you would like to create your Cordova project.
5. **cordova create "MyApp"**


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


**cordova platform add android**


**cordova platform add ios**


**cordova build** (Builds both platforms)


or 


**cordova build ios** / **cordova build android**


**cordova run ios** or **cordova run android**


By defualt the app will run on your device if it's connected. Otherwise it will run on the platfroms emulator.



_____________________
*Changing app icon.*
_____________________


Go to the config.xml file.


**icon src="res/icon.png"**


To use a different images for different platforms use:


**icon src="res/ios/icon.png" platform=“platform" width="57" height="57" density="mdpi"**


<https://cordova.apache.org/docs/en/latest/config_ref/images.html>


_____________________________________________________
*Splash Screen* 
_____________________________________________________

Adding a splash screen requires the following plugin:

**cordova plugin add cordova-plugin-splashscreen**


The splash resource goes in the config.xml file.


<https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-splashscreen/>
