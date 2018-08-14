
# TAILDMP SDK ANDROID

> The TailDMP SDK is a mobile data library used to send and collect data from mobile devices, which will be used by TailTarget. The library provides an automatization mechanism for collecting and sending data, it’s just necessary define the periodicity between the sending requests.  


## Required Settings 


#### File for Settings 
Create a file called TailDMPConfig.json, it must to be saved on the assets folder of the  project and needs to contain the accountId. 

Ex.

    {
      "accountId": "TT-0000-0"
    }

#### AndroidManifest Permissions
Add the following permissions to the  *AndroidManifest.xml*

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION" />

Add services below within the tag application

    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPCollectDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Collect Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendALLDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send All Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPActivityTrackerIntentService" android:exported="false" android:label="Tail DMP Activity Service" />

    
    
    
    


The file *AndroidManifest.xml* will look like this:


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

We must add to our gradle dependencies these libs and our .aar file: 
- play-services-identity:10.2.4
- play-services-location:10.2.4


Below is an example of how the dependency configuration of the *gradle.build* file should be:


Ex.
<pre>
dependencies {

    compile fileTree(dir: 'libs', include: ['*.jar'])

    //import google play libs
    <b>compile 'com.google.android.gms:play-services-identity:10.2.4'</b>
    <b>compile 'com.google.android.gms:play-services-location:10.2.4'</b>
    //get sdk from maven central
    <b>compile 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.22'</b>


}
</pre>




#### Bellow there is an example of how gradle.build file should be:

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
    compile 'com.google.android.gms:play-services-identity:10.2.4'
    compile 'com.google.android.gms:play-services-location:10.2.4'
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.android.support.constraint:constraint-layout:1.0.2'
    </b>
    //add and extract the sdk from mavencentral
    <b>compile 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.22'</b>


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





#### Request permission to capture geolocation data
To recover the device’s geolocation it is necessary to request the user’s permission. 
We can use the Helper provided by Android - ActivityCompat.requestPermissions to implement this workflow.
We recommends that this action should be executed before the start of SDK, on initializing of your app for example.
 
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

Reference: https://developer.android.com/training/permissions/requesting.html?hl=pt-br


#### Sending Data Without Automatic Scheduling
We can use the SDK to collect and send data of devices without the scheduling mechanism. 

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

                //Send data to webservice without a scheduled job Service, passing an enpty string as tag.
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
Add the following permissions to the AndroidManifest.xml


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

Add services below within the tag application


    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPCollectDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Collect Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPSendALLDataJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Tail DMP Send All Data Service"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.CheckNearByJobService"  android:permission="android.permission.BIND_JOB_SERVICE" android:label="Check beacon activity"/>
    <service android:name="digital.tail.sdk.tail_mobile_sdk.TailDMPActivityTrackerIntentService" android:exported="false" android:label="Tail DMP Activity Service" />


#### Graddle Configurations
For proper operation of the SDK, we use google play service libraries. They must be compiled within the app.

We must add to our gradle dependencies the following libraries: com.google.android.gms:play-services-nearby and play-services-location. 

Below an example of how the dependency configuration of the gradle.build file should be:


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
    compile 'com.google.android.gms:play-services-identity:11.0.4'
    <b>compile 'com.google.android.gms:play-services-location:11.0.4'
    compile 'com.google.android.gms:play-services-nearby:11.0.4'
    </b>

    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.android.support.constraint:constraint-layout:1.0.2'
    //add and extract the sdk from mavencentral
    <b>compile 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.22'</b>
}

</pre>





#### Enable beacons detection in your App 
Start the detection using the method **TailDMP.getInstance().enableBeaconScan()**.  

Beacon's detection and scan will be initialized only if user gives  <a href="#permission">permission</a> to access  device's geolocation and the bluetooth of its device is activated.

Every time we use enableBeaconScan(), the bluetooth of user's device is activated to allow us to search for beacons. 

Beacon's detection and scan are made when your App is in foreground and background state, this process runs in a separated thread in background, it doesn't blocks the UI thread and uses only **Bluetooth Low Energy (BLE)** to save device's battery.
 

Please don't forget to enable the automatic scheduling mechanism to send the data collected to us.  
- **TailDMP.getInstance().setIntervalToExecuteJob(minutes2collectr,minutes2Send)** defines periods to collect and send data. 
- **TailDMP.getInstance().startJob()** starts the process.

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
                //enable beacon tracking
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
            <b>TailDMP.getInstance().setIntervalToExecuteJob(15,480);
            TailDMP.getInstance().startJob();</b>

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
   <td ><pre>enableSandbox(boolean enable); </pre></td>
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
   <td ><pre>generateUserHashFromEmail(String useremail); </pre></td>
   <td>
    <p>Generate a hashuser by its email. Must be a lowercase String and a valid email address.</p>
   </td>
  </tr>
  
  <tr>
     <td ><pre>generateUserHashFromCPF(String CPF); </pre></td>
     <td>
      <p>Generates a hash user by its CPF, Must contents only 11 digits of this document.</p>
      <p>Ex. 111.111.111-11 => 11111111111</p>
      <p>Reference: https://dashboard.tailtarget.com/dmp/#/docs/a10</p>
     </td>
    </tr>
  
  <tr>
       <td ><pre>generateUserHashFromPhone(String phonenumber); </pre></td>
       <td>
        <p>Generates a hash user by its phonenumber, phone field, should use only the numbers of its full international form. Note that should be no international carrier codes in the phone's shape.</p>
        <p>Ex. 11982235000 => 5511982235000
8587719999 => 558587719999</p>
       <p>Reference: https://dashboard.tailtarget.com/dmp/#/docs/a10</p>
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
   <td ><pre>setSendDataOnWifiOnly(boolean); </pre></td>
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







  
  
  






 
