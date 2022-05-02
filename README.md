

# TAILDMP SDK ANDROID

> The TailDMP SDK is a mobile data library used to send and collect data from mobile devices, which will be used by TailTarget. The library provides an automatization mechanism for collecting and sending data, it’s just necessary define the periodicity between the sending requests.  


## Required Settings 


#### File for Settings 
Create a file called TailDMPConfig.json, it must to be saved on the assets folder of the  project and needs to contain the accountId. 

Ex.

    {
      "accountId": "TT-0000-0"
    }

#### AndroidManifest Permissions (android <=9)
Add the following permissions to the  *AndroidManifest.xml*

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION" />

#### AndroidManifest Permissions (android >=10)
In android versions <= 9 it was only necessary to ask permission for the user once to use location for both foreground and background.

Starting with version 10 of android, google has changed the way to request location permissions, for that it is necessary to use a permission flow in two steps, one for use in the foreground and tracking of activities and then for use of location in background.

Starting with  **version 11** there're new rules to use features like ACTIVITY_RECOGNITION , ACCESS_FINE_LOCATION
 , ACCESS_COARSE_LOCATION e ACCESS_BACKGROUND_LOCATION. 

User must be asked and allow the use of each feature and to get these data your app must have features that justifies its use.




If you're using **AndroidX**, some permissions has its package changed, see bellow comments about **android 10** use.


Add the following permissions to the  *AndroidManifest.xml*

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
     <!-- ACCESS_BACKGROUND_LOCATION is required only when requesting background location access on
      Android 10 (API level 29) and higher. -->
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
     <!-- ACTIVITY_RECOGNITION permission has its package changed 
     on Android 10 (API level 29) and higher. -->
    <uses-permission android:name="android.permission.ACTIVITY_RECOGNITION" />>


TailDMP SDK uses background location service, but for your app get it on android >=10 will need to grant access to foreground location service first.
More details on how to implement it can be found at:
https://developer.android.com/training/location/permissions#request-background-location

**We will not collect data linked to permissions if the user does not agree to the use. If your app doesn't need to use any of these permissions, don't add them in your AndroidManifest.**

Add services below within the tag application

    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPCollectDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Collect Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendALLDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send All Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPActivityTrackerIntentService" android:exported="false" android:label="Tail DMP Activity Service" />

    
    
    
    


The file *AndroidManifest.xml* will look like this for (android <=9):


    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" package="tail.digital.app" >
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION" />
        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/AppTheme" >
            <activity android:name=".MainActivity" >
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
            <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send Data Service"/>
            <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPCollectDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Collect Data Service"/>
            <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendALLDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send All Data Service"/>
            <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPActivityTrackerIntentService" android:exported="false" android:label="Tail DMP Activity Service" />
        </application>
    </manifest>


Or like this for **(android >=10)**:


    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" package="tail.digital.app" >
        <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <!-- Required only when requesting background location access on
     Android 10 (API level 29) and higher. -->
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <!-- ACTIVITY_RECOGNITION permission has its package changed 
     on Android 10 (API level 29) and higher. -->
    <uses-permission android:name="android.permission.ACTIVITY_RECOGNITION" />
        
        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/AppTheme" >
            <activity android:name=".MainActivity" >
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
            <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send Data Service"/>
            <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPCollectDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Collect Data Service"/>
            <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendALLDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send All Data Service"/>
            <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPActivityTrackerIntentService" android:exported="false" android:label="Tail DMP Activity Service" />
        </application>
    </manifest>




#### Add SDK Library in your project
The SDK library is distributed by mavencentral.

Our SDK Library already incorporates the following library: 
- com.google.code.gson:gson:2.8.0

If you already use it in your project, remove it from your gradle.build file to avoid possible compilation conflicts.


#### Graddle Configurations
For proper operation of the SDK, it is necessary to modify the project’s "build.gradle" file to compiles the Goolge play service and SDK libraries.

The Android minimum version required for the SDK's operation is **4.0 (api 14)**, 
the automatization mechanism for collecting and sending data is available only for Android **5.0 (api 20)** and up.

Ex.
<pre>
defaultConfig {
    applicationId "tail.digital.app"
    minSdkVersion <b>14</b>
    targetSdkVersion <b>25</b>
    versionCode 1
    versionName "1.0"
}
</pre>


For the correct operation of the SDK, we use google play service libraries. They must be compiled within the app. 

You must add these libs to your gradle dependencies: 
- play-services-identity:16.0.0
- play-services-location:16.0.0
- play-services-nearby:16.0.0

If your android app targets android > 11 you must add the lib play-services-ads on its version 16.0.0
- com.google.android.gms:play-services-ads:16.0.0

If you want to upgrade to a newer version of this lib you must include 
your Ad Manager app ID on AndroidManifest or you won't be able to compile.



Below is an example of how the dependency configuration of the *gradle.build* file should be:


Ex.
<pre>
dependencies {

    compile fileTree(dir: 'libs', include: ['*.jar'])

    //import google play libs
    <b>compile 'com.google.android.gms:play-services-identity:16.0.0'</b>
    <b>compile 'com.google.android.gms:play-services-location:16.0.0'</b>
    <b>compile 'com.google.android.gms:play-services-nearby:16.0.0'</b>

    //If your android app targets android > 11 you must add this lib on its version 16.0.0
    //If you want to upgrade to a newer version of this lib you must include 
    //your Ad Manager app ID on AndroidManifest or you won't be able to compile
    <b>implementation 'com.google.android.gms:play-services-ads:16.0.0'</b>

    //get sdk from maven central
    //change 1.2.+ to the latest version available
    <b>compile 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.+'</b>


}
</pre>




#### Bellow there is an example of how gradle.build file should be for android <= 9:

Ex.
<pre>
apply plugin: 'com.android.application'
android {
    compileSdkVersion 25
    buildToolsVersion "25.0.2"
    defaultConfig {
        applicationId "tail.digital.app"
        minSdkVersion <b>14</b>
        targetSdkVersion <b>25</b>
        versionCode 1
        versionName "1.0"

    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
       //import google play libs
    <b>
    compile 'com.google.android.gms:play-services-identity:16.0.0'
    compile 'com.google.android.gms:play-services-location:16.0.0'
    compile 'com.google.android.gms:play-services-nearby:16.0.0'
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.android.support.constraint:constraint-layout:1.0.2'
    </b>
    //add and extract the sdk from mavencentral
    //change 1.2.+ to the latest version available
    <b>compile 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.+'</b>


}
</pre>


#### Bellow there is an example of how gradle.build file should be for android >=10  AndroidX

Ex.
<pre>
plugins {
    id 'com.android.application'
}

android {
    compileSdkVersion <b>30</b>

    defaultConfig {
        applicationId "tail.digital.app"
        minSdkVersion <b>29</b>
        targetSdkVersion <b>30</b>
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {

    <b>
    //playservices on androidx has some packages changed
    implementation 'com.google.android.gms:play-services-identity:17.0.0'
    implementation 'com.google.android.gms:play-services-location:18.0.0'
    implementation 'com.google.android.gms:play-services-nearby:17.0.0'

    //If your android app targets android > 11 you must add this lib on its version 16.0.0
    //If you want to upgrade to a newer version of this lib you must include 
    //your Ad Manager app ID on AndroidManifest or you won't be able to compile
    implementation 'com.google.android.gms:play-services-ads:16.0.0'
    
    </b>

    //get sdk from maven central
    //change 1.2.+ to the latest version available
    implementation 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.+'
    
    //new androidx libs
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'com.google.android.material:material:1.1.0'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    testImplementation 'junit:junit:4.+'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'
}
</pre>


## Updating Security Providers of Google Play Services in Android 4.x 
Older devices with Android version 4.x installed, may be using obsolete and insecure libraries to provide secure network comunication. In these cases, we suggest that your app ask users to update Google Play Services Providers to its latest version.

Android's Google Developers Guide, describes how to do this operation here:
https://developer.android.com/training/articles/security-gms-provider.html  


**Warning: For security reasons, we'll collect and send data only if user's device security provider is up to dated**. 




## Using the SDK in a project
The SDK has a central feature access point named *TailDMP*. 
This object is a singleton and should be started only once, after passing the context of the application.



#### Setup and starting the SDK 
We must define some settings before start the SDK's automatic services of send and collect data.
After executing the required configurations, the SDK library must be started.

ex.
<pre>   
//Reference to a textfield on user interface
public TextView user_txtV;  

//Initialize the SDK
TailDMP.initialize(getApplicationContext());


//Generate an userhash ID using an email provided by a text field 
TailDMP.getInstance().generateUserHashFromEmail(user_txtV.getText().toString());

// Set user as optin <b>(required)</b>
TailDMP.getInstance().setOptin(true);

//interval, in minutes, to collect data from deivce 
int intervalToCollectData = 15;

//interval, in minutes, to send data to server 
int intervalToSendData = 120;
    
//Set interval that will fires up the collectDataJob and the sendDataJob <b>(required)</b>
TailDMP.getInstance().setIntervalToExecuteJob(intervalToCollectData,intervalToSendData);
</pre>


After setting up we can start the automatic service of collect and send data using the SDK's method *startJob()*.  
We use Android native runtime scheduling services (JobScheduler) to collect and send data.  These services **runs in foreground and background  states of app**.

    //Start the job
    TailDMP.getInstance().startJob();


You can initialize the job where you think is most adequate, in a click, in app's creation cycle, after app's initializing stage, etc.


Below an complete example of the setup of SDK and starting the job action of send and collect data.

Ex.
<pre>
package tail.digital.app;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import digital.tail.sdk.tail_mobile_sdk.TailDMP;
import digital.tail.sdk.tail_mobile_sdk.exception.TailDMPException;
public class MainActivity extends AppCompatActivity {
    
    //Reference to a textfield on user interface
    public TextView user_txtV;  
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initApp();
    }
    private void initApp() {
        try {
            //Initialize the SDK
            TailDMP.initialize(getApplicationContext());
            
            //Generate an userhash ID using an email provided by a text field 
            TailDMP.getInstance().generateUserHashFromEmail(user_txtV.getText().toString());
            
            // Set user as optin <b>(required)</b>
            TailDMP.getInstance().setOptin(true);
            
            //interval, in minutes, to collect data from deivce 
            int intervalToCollectData = 15;
            
            //interval, in minutes, to send data to server 
            int intervalToSendData = 120;
                
            //Set interval that will fires up the collectDataJob and the sendDataJob <b>(required)</b>
            TailDMP.getInstance().setIntervalToExecuteJob(intervalToCollectData,intervalToSendData);
        } catch (TailDMPException e) {                              
            e.printStackTrace();
        }
        
    }
        
    //clicks on a button to start the process
    public void startIt(View view){
        //Generate an userhash ID using an email provided by a text field 
            TailDMP.getInstance().generateUserHashFromEmail(user_txtV.getText().toString());

            //get the instance of TailDMP SDK and start automatic job  or send data 
             if (android.os.Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            
                //For Android 5.0 and greater  use this   
                TailDMP.getInstance().startJob();

            }else{
                
                //For Android 4.x use this
                TailDMP.getInstance().sendData("");
            
            }
    }
}
</pre>

Sometimes you need to stop the automatic job services to check or test some parts of apps workflow, you can do it using the method *stopJob()*.

You don't need to create triggers on your app's logic to stop the job service.

When you start the job services the SDK automatically handles its lifecycle, including start/stop/restart all services according with the period defined by **setIntervalToExecuteJob(intervalToCollectData,intervalToSendData)**.

**If you stop the service, for any reason, remember to restart it or we will never collect the data again.**

Ex.

    //Stop The job
    TailDMP.getInstance().stopJob();





### Request permission to capture geolocation data
To recover the device’s geolocation it is necessary to request the user’s permission. 
We can use the Helper provided by Android - ActivityCompat.requestPermissions to implement this workflow.
We recommends that this action should be executed before the start of SDK, on initializing of your app for example.

More details on how to implement it can be found at:
https://developer.android.com/training/permissions/requesting

If user don't grant permission to use its location the SDK will not use it.
 
Starting with version 10 of android, google has changed the way to request location permissions, for that it is necessary to use a permission flow in two steps, one for use in the foreground and tracking of activities and then for use of location in background.


**Starting with  Android 11 the permissions granted by the user will be revoked by default if the app is not used for a few months. 
In this case it is necessary to ask the user again to allow the use of his location or redirect the user to the configuration area where he can disable this behavior**

TailDMP SDK uses background location service, but for your app get it on android >=10 will need to grant access to foreground location service at first.
More details on how to implement it can be found at:
https://developer.android.com/training/location/permissions#request-background-location



Ex.  **(android <=9)**
<pre>
package tail.digital.app;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;

import digital.tail.sdk.tail_mobile_sdk.TailDMP;
import digital.tail.sdk.tail_mobile_sdk.exception.TailDMPException;

public class MainActivity extends AppCompatActivity {
    
    //Reference to a textfield on user interface
    public TextView user_txtV;  
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initApp();

        <b>//ask user to grant permissions
        askPermission()</b>
    }

    private void initApp() {

        try {

            //Initialize the SDK
            TailDMP.initialize(getApplicationContext());
            
            //Generate an userhash ID using an email provided by a text field 
            TailDMP.getInstance().generateUserHashFromEmail(user_txtV.getText().toString());
            
            // Set user as optin (required)
            TailDMP.getInstance().setOptin(true);
            
            //interval, in minutes, to collect data from deivce 
            int intervalToCollectData = 15;
            
            //interval, in minutes, to send data to server 
            int intervalToSendData = 120;
                
            //Set interval that will fires up the collectDataJob and the sendDataJob (required)
            TailDMP.getInstance().setIntervalToExecuteJob(intervalToCollectData,intervalToSendData);

        } catch (TailDMPException e) {                              
            e.printStackTrace();
        }
        
    }

    <b>private void askPermission(){

        //check if the user allow us to use some private resources of this device
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED ||
                ContextCompat.checkSelfPermission(this,Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED)
        {
            //We can add more than one  per requirement
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION,Manifest.permission.ACCESS_COARSE_LOCATION,Manifest.permission.RECEIVE_BOOT_COMPLETED},123);
        }       

    }</b>
        
    //clicks in a button to start the process
    public void startIt(View view){
        try {
          //get the instance of TailDMP SDK and start automatic job  or send data 
             if (android.os.Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            
                //For Android 5.0 and greater  use this   
                TailDMP.getInstance().startJob();

            }else{
                
                //For Android 4.x use this
                TailDMP.getInstance().sendData("");
            
            }
        }catch (TailDMPException e) {
            e.printStackTrace();
        }
    }
}
</pre>      

Reference: https://developer.android.com/training/permissions/requesting.html



Ex. Asking location permission flux in 2 steps **(android >=10)** .
<pre>


public class MainActivity extends AppCompatActivity {

    private  final int PERMISSION_REQUEST_ID = 123;
    private  final int PERMISSION_REQUEST_ID_BG = 456;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        <b>//ask user to grant permissions
        askPermission()</b>
    }

    public void <b>askPermission()</b>{
    //We can add more than one  per requirement
        ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACTIVITY_RECOGNITION, Manifest.permission.ACCESS_COARSE_LOCATION,Manifest.permission.ACCESS_FINE_LOCATION},PERMISSION_REQUEST_ID);
    }

    public void <b>askPermissionBG()</b>{
        //We can add more than one  per requirement
        ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_BACKGROUND_LOCATION},PERMISSION_REQUEST_ID_BG);
    }

@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {


    if(requestCode == PERMISSION_REQUEST_ID || requestCode == PERMISSION_REQUEST_ID_BG) {
        
        boolean foregroundLocAccess = false;
        boolean backgroundLocAccess = false;
        
        for(int i = 0; i < permissions.length; i++){
            if(permissions[i].equals("android.permission.ACCESS_COARSE_LOCATION")){
                foregroundLocAccess = true;
            }else if(permissions[i].equals("android.permission.ACCESS_BACKGROUND_LOCATION")){
                backgroundLocAccess = true;
            }
        }
        
        if(foregroundLocAccess){
            //permission foreground location granted, so ask for background location
            if(!backgroundLocAccess){
                <b>askPermissionBG()</b>;
            }
        }else{
            //permission foreground location denied, handle this case and ask again later
        }
    }

}   

}




</pre>


## Sending Data Without Automatic Scheduling
We can use the SDK to collect and send data of devices without the scheduling mechanism. 

**We suggest the use of this method when you don't need to use scheduling mechanism or when your app targets android version <5.** 
**You can call the method on some event of app like click, create , etc.** 



**We suggest the use of this method for all devices with Android 4 version installed**. 

If your user's device has Android 4 installed don't forget to ask him to update The Security Provider see [Updating Security Providers of Google Play Services in Android 4.x](#updating-security-providers-of-google-play-services-in-android-4-x)  

To execute this action we’ll use the method  **sendData(String tag)** at SDK. 

Note that is necessary to send a **tag** as parameter to this method. 
The **tag** must be a String and can not exceed 32 characters, it can be a empty string, can have letters, numbers or the following special characters: **-  _  .  =** 


Below an example of its use.

Ex.

    public class MainActivity extends AppCompatActivity {

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            initApp();
        }

        private void initApp() {

            try {

                //initializing TailDMP
                //Starts the SDK
                TailDMP.initialize(getApplicationContext());

                //Generate an userhash ID using an email provided by a text field 
                TailDMP.getInstance().generateUserHashFromEmail(user_txtV.getText().toString());
                                
                // Set user as optin (required)
                TailDMP.getInstance().setOptin(true);

                //Send data to webservice without a scheduled job Service, passing an empty string as tag.
                TailDMP.getInstance().sendData("");


            } catch (TailDMPException e) {
                e.printStackTrace();
            }
        }

    }


#### Checking data sent to Tailtarget using sandbox mode

To check all data that is being sent to Tailtarget, you must enable the "Sandbox mode" of SDK.  
This mode is disabled by default.

When enabled, all data sent by app are directed to a service that decrypt and display it on SDK SandBox area of Tailtarget's Dashboard. 
These data will not be processed because Sandbox mode proposal is to provide a interface to check if the SDK is sending data successfully and the configs are correct. 


The Sandbox area of Tailtarget's Dashboard can be acessed by Settings > Advanced Settings > SDK SandBox menu or directly by URL: https://dashboard.tailtarget.com/dmp/#/sandbox



**Warning! Before you publish your app, you must disable Sandbox mode or all data generated by your app will not be processed by TailTarget.** 


Below an example of how to enable Sandbox mode:

<pre>
    //initialize SDK
    TailDMP.initialize(getApplicationContext());
    //set user as optin
    TailDMP.getInstance().setOptin(true);
    
    //enable SANDBOX MODE
    // Don't forget to disable this mode before You publish the app
    <strong>TailDMP.getInstance().enableSandbox(true);
    </strong>
</pre>    


And how to disabled Sandbox mode:
<pre>
    //disable SANDBOX MODE
    <strong>TailDMP.getInstance().enableSandbox(false)</strong>
</pre>




#### Adding Tags to automatic process of collect and send data.
Tags are extra information which can to be sent to a webservice. 
This information must be a String and can not exceed 32 characters, can not be an empty string, can have letters, numbers or the following special characters: **-  _  .  =** 

When a tag is added to SDK, a new register of data will be collected and sent to us.

Ex.

    //We must get the instance initialized
    try{
        tDMP = TailDMP.getInstance();
    }catch (TailDMPException e) {
        e.printStackTrace();
    }
    
    String mytag = "onlineChannel";

    if(tDMP != null)
    tDMP.addTags(mytag);




## BEACONS

#### Configure Beacons to be detected by TailDMP SDK
Our library can be used to detect your beacons **(For Android api >=21 only)**.

The SDK supports <a href="https://developers.google.com/beacons/eddystone" target="_blank"><b>Eddystone-UID</b></a> 
protocol that broadcasts an unique message identifier for each beacon.

The identifier is splitted into two parts, _Namespace_ (10 bytes) e  _Instance_ (6 bytes).


The Namespace is used to filter your beacons from other people's beacons and Instance to identify and differentiate between each one of yours beacons.

To be detected by our SDK , your beacon message **Namespace** must be setted to **CFB3DBE6A3E1B065C4BB**, we only track beacons with this Namespace.


The **Instance** value can be represented as a 12 characters long string. ex: 112233445566, 112233445567.

We use Google's "Nearby" library to control the process of beacon's scanning. For a proper operation, the geolocation service must be enabled by user.

These scans are triggered by screen ON/OFF events. 

To detect Beacons the bluetooth of device must be activated , we recommend that you create strategies that encourages your user to keep the bluetooth on. 


## Configure SDK to detect beacons
### Required Settings 

#### AndroidManifest Permission
Add the following permissions to the AndroidManifest.xml for **android <= 9**


    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

    <uses-permission android:name='android.permission.BLUETOOTH'/>
    <uses-permission android:name='android.permission.BLUETOOTH_ADMIN'/>
    <uses-feature android:name="android.hardware.bluetooth_le" android:required="true"/>

    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />



 Or the following permissions to the AndroidManifest.xml for **android >=10** using AndroidX


    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <!-- Required only when requesting background location access on
     Android 10 (API level 29) and higher. -->
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />

    <uses-permission android:name='android.permission.BLUETOOTH'/>
    <uses-permission android:name='android.permission.BLUETOOTH_ADMIN'/>
    <uses-feature android:name="android.hardware.bluetooth_le" android:required="true"/>

    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <!-- ACTIVITY_RECOGNITION permission has its package changed 
     on Android 10 (API level 29) and higher. -->
    <uses-permission android:name="android.permission.ACTIVITY_RECOGNITION" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />




Add services below within the tag application


    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPCollectDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Collect Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendALLDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send All Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.CheckNearByJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Check beacon activity"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPActivityTrackerIntentService" android:exported="false" android:label="Tail DMP Activity Service" />


#### Graddle Configurations
For proper operation of the SDK, we use google play service libraries. They must be compiled within the app.

We must add to our gradle dependencies the following libraries: com.google.android.gms:play-services-nearby and play-services-location. 

Below an example of how the dependency configuration of the gradle.build for **android <=9** file should be:


<pre>

apply plugin: 'com.android.application'
android {
    compileSdkVersion 25
    buildToolsVersion "25.0.2"
    defaultConfig {
        applicationId "tail.digital.app"
        minSdkVersion 21
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"

    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    
    //import google play libs
    <b>compile 'com.google.android.gms:play-services-identity:16.0.0'
    compile 'com.google.android.gms:play-services-location:16.0.0'
    compile 'com.google.android.gms:play-services-nearby:16.0.0'
    </b>

    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.android.support.constraint:constraint-layout:1.0.2'
    //add and extract the sdk from mavencentral
    //change 1.2.+ to the latest version available
    <b>compile 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.+'</b>
}

</pre>




Below an example of how the dependency configuration of the gradle.build for **android >=10** with AndroidX  should be:

Ex.
<pre>
plugins {
    id 'com.android.application'
}

android {
    compileSdkVersion <b>30</b>

    defaultConfig {
        applicationId "tail.digital.app"
        minSdkVersion <b>25</b>
        targetSdkVersion <b>30</b>
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {

    <b>
    //playservices on androidx has some packages changed
    implementation 'com.google.android.gms:play-services-identity:17.0.0'
    implementation 'com.google.android.gms:play-services-location:18.0.0'
    implementation 'com.google.android.gms:play-services-nearby:17.0.0'
    //If your android app targets android > 11 you must add this lib on its version 16.0.0
    //If you want to upgrade to a newer version of this lib you must include 
    //your Ad Manager app ID on AndroidManifest or you won't be able to compile
    implementation 'com.google.android.gms:play-services-ads:16.0.0'
    
    
    </b>

    //get sdk from maven central
    //change 1.2.+ to the latest version available
    implementation 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.+'
    
    //new androidx libs
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'com.google.android.material:material:1.1.0'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    testImplementation 'junit:junit:4.+'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'
}
</pre>




#### Enable beacons detection in your App 
Beacon's detection is enabled automatically using the method of SDK, **TailDMP.getInstance().startJob()**. 
  
You can start the detection manually using the SDK's method **TailDMP.getInstance().enableBeaconScan()**. 
To stop the detection use the SDK's method **TailDMP.getInstance().disableBeaconScan()**.

Beacon's detection and scan will be initialized only if user is opt in, gives  <a href="#permission">permission</a> to access  device's geolocation and the bluetooth of its device is activated.

All beacon's information are saved inside app and are sent to Tail using SDK's service that sends and collects data automaticcally, so you **must use the SDK on automatic mode**.


Beacon's detection and scan are executed when your App is in foreground and background state, this process runs in a separated thread in background, it doesn't blocks the UI thread and uses only **Bluetooth Low Energy (BLE)** to save device's battery.



Below an example of enabling beacons detection, the workflow to ask permission to use geolocation data and enabling automatic scheduling mechanism to send and collect data:


<pre>
package digital.tail.apptrackbeacons;

import android.Manifest;
import android.content.pm.PackageManager;
import android.graphics.Color;
import android.support.annotation.NonNull;
import android.support.design.widget.Snackbar;
import android.support.v4.app.ActivityCompat;
import android.support.v4.content.ContextCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import com.google.android.gms.common.api.GoogleApiClient;
import com.google.android.gms.nearby.messages.MessageListener;

import digital.tail.sdk.tail_mobile_sdk.TailDMP;
import digital.tail.sdk.tail_mobile_sdk.TailDMPValues;
import digital.tail.sdk.tail_mobile_sdk.exception.TailDMPException;


//We need to handle PermissionsResultCallback so we implement its methods in this activity
public class MainActivity extends AppCompatActivity implements ActivityCompat.OnRequestPermissionsResultCallback{

    private int PERMISSION_REQUEST_ID = 123;

    TextView txt_email;

    Button btEnablebeacon;
    Button btDisablebeacon;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //get email textfield
        txt_email = (TextView)findViewById(R.id.txt_email);


        //get beacons bts
        btEnablebeacon = (Button) findViewById(R.id.btEnablebeacon);
        btDisablebeacon = (Button) findViewById(R.id.btDisablebeacon);

        //setcolors
        btEnablebeacon.setBackgroundColor(Color.parseColor("#4caf50"));
        btEnablebeacon.setTextColor(Color.parseColor("#FFFFFF"));

        btDisablebeacon.setBackgroundColor(Color.parseColor("#f44336"));
        btDisablebeacon.setTextColor(Color.parseColor("#FFFFFF"));


        //initialize TailDMP SDK
        initApp();
    }



    /****************************
     *
     * Initializing Tal DMP SDK
     *
     ***************************/
    private void initApp(){

        //initializing TailDMP

        try {
            TailDMP.initialize(getApplicationContext());
            //set user as optin
            TailDMP.getInstance().setOptin(true);


        } catch (TailDMPException e) {
            e.printStackTrace();
        }


    }


    /****************************
     *
     * Initializing Beacons tracking
     *
     ***************************/
    public void initBeacons(View view){

        //just initialize if we have location permission granted
        if (ContextCompat.checkSelfPermission(getApplicationContext(), Manifest.permission.ACCESS_FINE_LOCATION)
                == PackageManager.PERMISSION_GRANTED) {
            //init beacons tracking
            try {
                
                //generate user hash
                TailDMP.getInstance().generateUserHashFromEmail(txt_email.getText().toString());
                <b>
                //enable beacon tracking manually
                TailDMP.getInstance().enableBeaconScan(this);
                </b>

                /****************************************************************************
                 *
                 * Don't forget to start the service to automatically collect and send data
                 * or it will never be sent to be processed by us.
                 *
                *****************************************************************************/
                //collect and send data
                startSendDataService();

            } catch (TailDMPException e) {
                e.printStackTrace();
            }
        }else{
            //ask permission

            View v = findViewById(android.R.id.content);
            checkPermission(v);
        }
    }


    /****************************
     *
     * Stops Beacons tracking
     *
     ***************************/
    public void disableBeacons(View v){

        try {
            //stops tracking beacons
            TailDMP.getInstance().disableBeaconScan();

            


        } catch (TailDMPException e) {
            e.printStackTrace();
        }


    }

    /****************************
     *
     * Stars automatic service that collect and send data
     *
     ***************************/
    private void startSendDataService(){
        try {
            //every 15 minutes try to collect data
            //every 480(8 hours) minutes try to send data to server
            <b>TailDMP.getInstance().setIntervalToExecuteJob(15,480);</b>
            //initialize beacon tracking automatically
            <b>TailDMP.getInstance().startJob();</b>

        } catch (TailDMPException e) {
            e.printStackTrace();
        }
    }


    /****************************
     *
     * Stops automatic service that collect and send data
     *
     ***************************/
    private void stopSendDataService(){
        try {

            TailDMP.getInstance().stopJob();

        } catch (TailDMPException e) {
            e.printStackTrace();
        }
    }



    // =============================
    // ==>> ASK PERMISSION TO USER
    // =============================
    //https://developer.android.com/training/permissions/requesting.html



    /****************************
     *
     * Ask permission dialog for user interaction
     *
     ***************************/
    public void checkPermission(View view){
        //check if the user allow us to use some private resources of this device
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED ||
                ContextCompat.checkSelfPermission(this,Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED
                ){
            //Can add more as per requirement
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION,Manifest.permission.ACCESS_COARSE_LOCATION},PERMISSION_REQUEST_ID);
        }
    }


    /****************************
     *
     * Handler of Dialog's action
     *
     ***************************/
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {

        if (requestCode == PERMISSION_REQUEST_ID) {

            int PERMISSION_GRANTED = -1;

            //loop through all permissions requested
            for(int i = 0; i<permissions.length; i++){

                //found ACCESS_COARSE_LOCATION
                if(permissions[i].equals("android.permission.ACCESS_COARSE_LOCATION")){
                    PERMISSION_GRANTED = grantResults[i];
                    break;
                }

            }

            // Check if the required permission has been granted
            if (PERMISSION_GRANTED == PackageManager.PERMISSION_GRANTED) {
                <b>// access to locations device granted
                //start beacon tracking
                initBeacons(null);
                </b>
            } else {

                Snackbar.make(this.findViewById(android.R.id.content), "You must grant permission to access device location if you want to use this feature!",
                        Snackbar.LENGTH_LONG).show();

            }

        }else{
            super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        }


    }
}


</pre>


## Events

Events are key / value data that can be saved when an in-app action occurs. ex: User clicked to buy a product, User cancel a purchase, etc.

Users must be opt in to its data be saved. The maximum number of saved events is **400**. 

After reaching the maximum value of events we start to  delete the oldest record to insert a new one. 

**Its up to the developer to trigger the operation of sending saved event data.**

### Save Events 

To save events call the method **addEvent(eventKey,eventValue)**.

The value of **eventKey** must be a String up to 32 characters, cannot be null, cannot be empty and can contain _=.-

The value of **eventValue** must be a String up to 100 characters, cannot be null, cannot be empty and can contain _=.-

Ex:
<pre>


    //user clicks and we add an event to be saved
    public void clickAddEvent (View view) {
        //adding event
        try{
           <b> TailDMP.getInstance().addEvent("_viewProduct", "sku_1");</b>
        }catch(Exception e){
            Log.e("ERROR", "Exception");
        }
    }
</pre>


### Upload Events 
The operation of uploading events to TailTarget is initiated by calling the method  **sendEvents**.

This method returns true if the operation has successfully started or false if a failure has occurred.

You can only perform one upload operation at a time, 
if a sequence of calls is needed you must implement a callback
to handle responses of one call and initiate the next one. 
 
During the operation of uploading events, requests are executed to our servers transmitting all data saved.

If all requests are successfully completed the value true is returned to callback, else the value false is returned.

If a callback has not been defined, nothing is returned. 

The data sent is deleted from the app with each successful submission request. 

If any request was not completed, the data that was being sent by it will be kept in the app.

In this case, an action of developer is required to schedule a new upload attempt

#### Uploading Events with callback
Uploading events data is an asynchronous operation, we receive a response of it after this operation has completed.

To listen to this response we must use a callback method that is executed after this operation is completed.

Tail SDK already has an implementation for this purpose.  
The developer must implements in your Activity the method 
**sendEventsResults(boolean data)** of SDK's interface 
**ITSendEvents** .

The SDK will use this interface as callback and will return **true** if the request was successfully completed or **false** to any failure. 



Ex:
<pre>
public class MainActivity extends AppCompatActivity <b>implements ITSendEvents</b> {

    //user clicks to send events
    public void clickSendAllEvents(View view){
        try{

           /* 
            Sending all saved events to tailtarget server.
            We had implemented on this Activity the method sendEventsResults() from ITSendEvents 
            and then passed "this" as callback on sendEvents(this).
            */
            boolean sendEventsOperation = <b>TailDMP.getInstance().sendEvents(this)</b>;

            if(sendEventsOperation){
                Log.i("OK", "Operation Initialized!");
            }else{
                Log.e("ERROR", "Operation NOT Initialized!");
                // do some task to handle this.
            }
        }catch (Exception e){
            Log.e("ERROR", "Exception");
        }
    }


    //user clicks and we add an event to be saved
    public void clickAddEvent (View view) {
        
        try{

            TailDMP.getInstance().addEvent("_viewProduct", "sku_1");
        }catch(Exception e){
            Log.e("ERROR", "Exception");
        }

    }

    //callback method
    @Override
    public void sendEventsResults(boolean b) {
        Log.i("TAIL-TAG", "Response from sendEvents"+b);
    }
}
</pre>

#### Upload Events without callback
The use of the callback is not mandatory, if you don't need it, pass null to the sendEvents method ex: sendEvents (null)


Ex:
<pre>

    public void clickSendAllEvents(View view){
        try{

           /* 
            Sending all saved events to tailtarget server without callback.
            */
            boolean sendEventsOperation = <b>TailDMP.getInstance().sendEvents(null);</b>

            if(sendEventsOperation){
                Log.i("OK", "Operation Initialized!");
            }else{
                Log.e("ERROR", "Operation NOT Initialized!");
                // do some task to handle this.
            }
        }catch (Exception e){
            Log.e("ERROR", "Exception");
        }
    }
</pre> 





## Class Summary

### TailDMP

> Central access object of the SDK features. 
> This object is a singleton and should be initialized only once, after the application context.

<table >
 <thead>
  <tr>
   <th colspan="2">Constructor</th>
  </tr>
 </thead>

 <tbody>
  <tr>
   <td>
    <pre>initialize(Context context)</pre>
  </td>
  <td>
   <p>Starting method of Singleton TailDMP, must to be done only once. 
Mandatory.
</p>
  </td>
 </tr>
 </tbody>
</table>

<table>  
 <thead>
  <tr>
  <th colspan="2">Public Methods</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td ><pre>enableSandbox(boolean enable) </pre></td>
   <td>
    <p>Enabel/Disable the Sandbox mode. </p>
    <p>The sandbox mode is disabled by default.</p>
    <p>
        When enabled, all data is sent to a service that decrypt and display it on Sandbox area of Dashboard.  
    </p>
    <p>
        The sandbox mode is used to check if your app config is correct and the data is been sent to TailTarget services.
    </p>
   </td>
  </tr>


  <tr>
   <td ><pre>getInstance() </pre></td>
   <td>
    <p>Retrieves the initialized TailDMP instance</p>
   </td>
  </tr>  
  <tr>
   <td ><pre>generateUserHashFromEmail(String useremail) </pre></td>
   <td>
    <p>Generate a hashuser by its email. Must be a lowercase String and a valid email address.</p>
   </td>
  </tr>
  
  <tr>
     <td ><pre>generateUserHashFromCPF(String CPF) </pre></td>
     <td>
      <p>Generates a hash user by its CPF, Must contents only 11 digits of this document.</p>
      <p>Ex. 111.111.111-11 => 11111111111</p>
      <p>Reference: https://dashboard.tailtarget.com/dmp/#/docs/a10</p>
     </td>
    </tr>
  
  <tr>
       <td ><pre>generateUserHashFromPhone(String phonenumber) </pre></td>
       <td>
        <p>Generates a hash user by its phonenumber, phone field, should use only the numbers of its full international form. Note that should be no international carrier codes in the phone's shape.</p>
        <p>Ex. 11982235000 => 5511982235000
8587719999 => 558587719999</p>
       <p>Reference: https://dashboard.tailtarget.com/dmp/#/docs/a10</p>
       </td>
      </tr>
  <tr>
   <td ><pre>addEvent(String eventKey, String eventValue) </pre></td>
   <td>
    <p>Saves key / value data on app.</p>
    <p><b>eventKey</b> - must be a String up to 32 characters, cannot be null, cannot be empty and can contain _=.-</p>
<p><b>eventValue</b> must be a String up to 100 characters, cannot be null, cannot be empty and can contain _=.-</p>
   </td>
  </tr>  
  <tr>
   <td ><pre>addTags(Strig tag) </pre></td>
   <td>
    <p>Adds extra information to be sent to server</p>
    <p><b>tag</b> - must be a String and can not exceed 32 characters, <b>can not</b> be an empty string, can have letters, numbers or the following special characters: **-  _  .  =** </p>
   </td>
  </tr>  
  <tr>
   <td ><pre>sendData(String tag) </pre></td>
   <td>
    <p>Sends device data without automatic JobService </p>
    <p><b>tag</b> - String and can not exceed 32 characters, <b>can</b> be an empty string, can have letters, numbers or the following special characters: **-  _  .  =** </p>
   </td>
  </tr>
  <tr>
   <td ><pre>sendEvents(ITSendEvents listener) </pre></td>
   <td>
    <p>Operation that upload events data</p>
    <p>Returns true if the operation has successfully started or false if a failure has occurred.</p>
    <p><b>listener</b> Object with the implementation of callback of interface ITSendEvents. It will return true if the request was successfully completed or false to any failure.</p><p>The use of the callback is not mandatory, if you don't need it, pass null to the sendEvents method ex: sendEvents (null)</p>
   </td>
  </tr>
  <tr>
   <td ><pre>setIntervalToExecuteJob(int minutesToCollect, int minutesToSend) </pre></td>
   <td>
   <p><b>(Required)</b> Adds a time interval between the Service’s 
execution which collects and sends data to the server
</p>
    <p><b>minutesToCollect</b> - Adds a time interval, in minutes, to collect device data. The minimum period between collect service must 15 minutes</p>
    <p><b>minutesToSend</b> - Adds a time interval, in minutes, to send device data to our server . The minimum period between send data service must be bigger than <b>minutesToCollect</b>  and its maximum value up to 24hs(1440 minutes) </p>
   </td>
  </tr>
  <tr>
   <td ><pre>startJob() </pre></td>
   <td>
    <p>Starts the service of periodic scheduling of collect and sending 
data, respecting time interval settled 
by  setIntervalToExecuteJob(int minutesToCollect, int minutesToSend)</p>
   </td>
  </tr>
  <tr>
   <td ><pre>stopJob() </pre></td>
   <td>
    <p><b>(Optional)</b> Stops the execution of automated service that collects and sends data to our server. </p>
   </td>
  </tr>
  
  <tr>
     <td ><pre>setOptin(boolean) </pre></td>
     <td>
      <p><b>(Required)</b> true | false - Allows (or not) the sdk to collect and send data, default is always true.</p>
     </td>
    </tr>  
  
  <tr>
   <td ><pre>setSendDataOnWifiOnly(boolean) </pre></td>
       <td>
        <p><b>(Optional)</b> true | false - Signals to the SDK that the sending of data will only be done if the device have data connection through Wifi network , default is false.</p>
       </td>
      </tr>
<tr>
   <td ><pre>enableBeaconScan(Activity act) </pre></td>
   <td>
    <p>
        Enable Beacons tracking and detection.
    </p>
   </td>
  </tr>
<tr>
   <td ><pre>disableBeaconScan() </pre></td>
   <td>
    <p>
        Disable Beacons tracking and detection.
    </p>
   </td>
  </tr>  

 </tbody>
</table>







  
  
  






 
