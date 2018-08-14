# TAILDMP SDK ANDROID

> The TailDMP SDK is a mobile data library used to send and collect data from mobile devices, which will be used by TailTarget. The library provides an automatization mechanism for collecting and sending data, it’s just necessary define the periodicity between the sending’s requests.  


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
- tail-mobile-sdk.aar file

Below is an example of how the dependency configuration of the *gradle.build* file should be:


Ex.
<pre>
dependencies {


	//import google play libs
	<b>compile 'com.google.android.gms:play-services-identity:10.2.4'</b>
	<b>compile 'com.google.android.gms:play-services-location:10.2.4'</b>
	//add and extract the sdk to
	//get sdk from maven central
	<b>compile 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.21'</b>

}
</pre>



#### Bellow there is an example of how the gradle.build file should be:

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
	<b>compile 'digital.tail.sdk.tail_mobile_sdk:tail-mobile-sdk:1.2.21'</b>

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
Bellow, an exemplo of how to setup SDK for Android 5.0 and up, for Android 4.x see [Sending Data Without Automatic Scheduling](#sending-data-without-automatic-scheduling).

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


You can initialize the job where you think is most adequed, in a click, in app's creation cycle, after app's initializing stage, etc.


Bellow an complete example of the setup of SDK and starting the job action of send and collect data for Android 4.0 and up.

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

			if (android.os.Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {    
            //Set interval that will fires up the collectDataJob and the sendDataJob <b>(required)</b>
            TailDMP.getInstance().setIntervalToExecuteJob(intervalToCollectData,intervalToSendData);
			}

        } catch (TailDMPException e) {                              
            e.printStackTrace();
        }
        
    }
        
    //clicks on a button to start the process
    public void startIt(View view){
        try {

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


        }catch (TailDMPException e) {
            e.printStackTrace();
        }
    }
}
</pre>

Sometimes you need to stop the automatic job services to check or test some parts of apps workflow, you can do it using the method *stopJob()*.

You don't need to create triggers on your app's logic to stop the job service.

When you start the job services the SDK automatically handles its lifecycle, including start/stop/restart all services according with the period defined by **setIntervalToExecuteJob(intervalToCollectData,intervalToSendData)**.

**If you stop the service, for any reason, remember to restar it or we will never collect the data again.**

Ex.

	//Stop The job
	TailDMP.getInstance().stopJob();





#### Request permission to capture geolocation data
To recover the device’s geolocation it is necessary to request the user’s permission. 
We can use the Helper provided by Android - ActivityCompat.requestPermissions to implement this workflow.
We recomends that this action should be executed before the start of SDK, on initializing of your app for example.
 
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
            //get the instance of TailDMP SDK and start the job
            TailDMP.getInstance().startJob();
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

              


			} catch (TailDMPException e) {
				e.printStackTrace();
			}
		}

		private void clickButton(View view){
			
			try {
			  //Send data to webservice without a scheduled job Service, passing an enpty string as tag.
                TailDMP.getInstance().sendData("");
			}catch (TailDMPException e) {
            	e.printStackTrace();
        	}

		}

	}


#### Checking data sent to Tailtarget using sandbox mode

To check all data that is being sent to Tailtarget, you must enable the "Sandbox mode" of SDK.  
This mode is disabled by default.

When enabled, all data sent by app are directed to a service that decrypt and display it on SDK SandBox area of Tailtarget's Dashboard. 
These data will not be processed because Sandbox mode proposal is to provide a interface to check if the SDK is sending data successfully and the configs are correct. 


The Sandbox area of Tailtarget's Dashboard can be acessed by Settings > Advanced Settings > SDK SandBox menu 



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




#### Adding Tags to automatic proccess of collect and send data.
Tags are extra information which can to be sent to a webservice. 
This information must be a String and can not exceed 32 characters, can not be a empty string, can have letters, numbers or the following special characters: **-  _  .  =** 

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
      <p>Reference: https://dashboard.tailtarget.com/docs/a10</p>
     </td>
    </tr>
  
  <tr>
       <td ><pre>generateUserHashFromPhone(String phonenumber); </pre></td>
       <td>
        <p>Generates a hash user by its phonenumber, phone field, should use only the numbers of its full international form. Note that should be no international carrier codes in the phone's shape.</p>
        <p>Ex. 11982235000 => 5511982235000
8587719999 => 558587719999</p>
       <p>Reference: https://dashboard.tailtarget.com/docs/a10</p>
       </td>
      </tr>
  
  <tr>
   <td ><pre>addTags(Strig tag) </pre></td>
   <td>
    <p>Adds extra information to be sent to server</p>
    <p><b>tag</b> - must be a String and can not exceed 32 characters, <b>can not</b> be a empty string, can have letters, numbers or the following special characters: **-  _  .  =** </p>
   </td>
  </tr>  
  <tr>
   <td ><pre>sendData(String tag) </pre></td>
   <td>
    <p>Sends device data without automatic JobService </p>
    <p><b>tag</b> - String and can not exceed 32 characters, <b>can</b> be a empty string, can have letters, numbers or the following special characters: **-  _  .  =** </p>
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
  

 </tbody>
</table>







  
  
  






 
