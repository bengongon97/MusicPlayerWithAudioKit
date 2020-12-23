## Table of Contents

 * [Introduction](#introduction)
 * [Installation](#installation)
 * [Configuration ](#configuration )
 * [Supported Environments](#supported-environments)
 * [Sample Code](#Sample-Code)
 * [License](#license)
 
## Introduction
    HMS Audio Kit Codelab code encapsulates APIs of the HUAWEI Audio Kit SDK. It provides many sample programs for your reference or usage.
    Before you use this codelab, it's assumed that you already have a HUAWEI developer account and have already created an app to implement the HMS Video Kit. If you haven't, please refer to https://developer.huawei.com/consumer/en/doc/start/introduction-0000001053446472 and https://developer.huawei.com/consumer/en/doc/development/AppGallery-connect-Guides/agc-introduction.
    
    Adapters:           The package which contains the adapters for RecyclerViews scattered throughout the app.
    UtilsAndServices:   The package which contains the necessary utils and services to provide smooth user experience.
    
## Installation
    Before using HMS Audio Kit Codelab code, check whether the Android Studio environment has been installed. 
    Download the HMS Audio Kit Codelab project by zip or clone in Github.
    Wait for the gradle build in your project.
    
## Supported Environments
	•	Android Studio 3.x or later version
	•	Java JDK 1.7 or later version
	•	EMUI 4.0 or later version
	•	SDK Platform 19 or later
	•	HMS Core (APK) 5.0.0.300 or later version
  
## Configuration 
    1. Register and sign in to HUAWEI Developers.
    2. Create a project and then create an app in the project, enter the project package name.
    3. Go to Project Settings > Manage APIs, find the Video Kit API, and enable it.
    4. Go to Project Settings > General information, click Set next to Data storage location under Project information, and select a data storage location in the displayed dialog box.
    5. Download the agconnect-services.json file and place it to the app's root directory of the project.
    6. Add the Maven repository address maven {url 'https://developer.huawei.com/repo/'} and plug-in class path 'com.huawei.agconnect:agcp:1.4.1.300' to the project-level build.gradle file.
    7. Add apply plugin: 'com.huawei.agconnect' to the last line of the app-level build.gradle file.
    8. Configure the dependency com.huawei.hms:audiokit-player:1.1.0.300 in the app-level buildle.gradle file.
    9. Synchronize the project.
    
## Sample Code
    HMS Audio Kit Codelab code contains many functions to implement many features. Following is a summary of some crucial methods.
    1) Initiliaze HwAudioManager instance.
    You can initialize HwAudioManager and other sub-managers in AsyncTask method.
    Code location src/main/java/com.hms.codelab.audiokit.musicplayerwithaudiokit/AudioActivity.kt
    
    2) Implement listeners
    You can obtain the implemented listener object and attach it to the managers to control the playback
    Code location  src/main/java/com.hms.codelab.audiokit.musicplayerwithaudiokit/AudioActivity.kt
    
    3) Playing Local Audio
    You can play local audio. Your storage will be scanned automatically and audio files will be brought up.
    Code location src/main/java/com.hms.codelab.audiokit.musicplayerwithaudiokit/AudioActivity.kt
    
    4) Playing Online Audio
    You can preset online audio. Audio files that will be selected can be configured in the code.
    Code location src/main/java/com.hms.codelab.audiokit.musicplayerwithaudiokit.UtilsAndServices/PlaylistCreator.kt
    
    5) Using Other Video Kit Features
    Many features of the Audio Kit can be experienced throughout the sample app. Basic and advanced playback exists in music play screen.
    Code location src/main/java/com.hms.codelab.audiokit.musicplayerwithaudiokit/AudioActivity.kt
    
    6) Using notification bar
    Audio playback can be controlled from notification bar in real time.
    Code location src/main/java/com.hms.codelab.audiokit.musicplayerwithaudiokit/AudioActivity.kt

## License
    HMS Audio Kit Codelab is licensed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0).
