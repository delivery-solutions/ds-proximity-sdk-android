  

Proximity Android SDK - Permissions Explained
============================================================

### 1.1 Internet

`android.permission.INTERNET

Internet permission is need to call external Api’s and to sync data with Proximity platform.

### 1.2 Location - Fine or Coarse

    android.permission.ACCESS_FINE_LOCATION | android.permission.ACCESS_COARSE_LOCATION 

Inorder to get the current location of the users device and update them Proximity platform.

### 1.3 Background Location Access

    android.permission.ACCESS_BACKGROUND_LOCATION

This permission enables the SDK to get access to geo location even when the app is minimsed. Please note that the SDK should recieve "All the time" permission to work properly.

### 1.4 Foreground Service

    android.permission.FOREGROUND_SERVICE

Proximity SDK will create a forground service so that device location can be accessed in background. This service permission will enable the SDK to work even if the application is terminated by the user.

> Copy and paste below service permoission code into your app manifest file.
```xml
    <service android:name="co.deliverysolutions.proximity.location.LocationService"
                  android:enabled="true"
                  android:exported="false"/>
```                

### 1.5 User Activity Recognition

    android.permission.ACTIVITY_RECOGNITION

This permission allows the SDK to understand what kind of activity is the user performing in a given moment. Example { **‘Still’**, **‘In-Vehicle’**, **‘Walking’**, **‘On-Bicycle’**, **‘Running’**} . This will be useful to predict the user ETA for store pick up scenarios.

> Copy and paste this receiver code to your app Manifest file to provide access for user-activity

```xml
    <receiver android:name="co.deliverysolutions.proximity.UserActivity$ActivityTransitionReceiver"
                  android:exported="false"
                 android:permission="com.google.android.gms.permission.ACTIVITY_RECOGNITION"/>  
```
