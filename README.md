# Sojourn
Sojourn is a flexible location collection service. It comprises of a mobile SDK for Android or iOS and a web based dashboard to manage your deployment. A core feature of Sojourn is it's efficient algorithm for collecting location data. Location services are not used until the device moves significantly.

The web dashboard provides a portal to manage your deployment. You can create and manage geofences and measure users dwell time at these locations. Basic reporting is available.

We recognise that every use case that requires location from a mobile device is different. With this in mind the system is highly customisable, giving developers the ability to trigger actions on data updates, and easily persist data in the cloud.

Features
* iOS & Android native SDKs
* Quick and easy integration.
* Power efficient algorithm for collecting location data.
* Highly customisable.
* Supports offline store and forward algorithms.
* JavaScript SDK available.
* Support for iBeacons.
* Support for circular geofences.
## Other features:
* Flexible cloud rules engine.
* Inbox
* Social Feeds
* Deep linking.
* System can support user sign-up and anonymous users.
* Email and SMS MFA.
* Web Dashboard
* Create and manage geofences
* Create and manage iBeacons
* Basic reporting active users, visits
* Export of location data
* Server support before and after save triggers.
* Custom rule logic can be implemented.
* Push notifications.
* Highly scalable system.

# Getting Started

### Android Studio Integration

Add this repository to the *project*-level build.gradle file:

```
allprojects {
    repositories {
        maven{ 
            url "https://raw.githubusercontent.com/MelroseCS/sojourn-android/stable/" 
        }
    }
}
```

Add the sojourn library and related dependencies to the *module*-level build.gradle file:

```
dependencies {
    implementation 'com.melrose.sojourn:sojourn:0.0.1@aar'
    implementation 'com.android.support:appcompat-v7:27.1.1'
    implementation 'com.android.support:support-v4:27.1.1'
    implementation 'com.google.android.gms:play-services-location:15.0.1'
    implementation 'com.google.android.gms:play-services-maps:15.0.1'
    implementation 'com.parse:parse-android:1.17.3'
    implementation 'com.parse:parse-fcm-android:1.17.3'
    implementation 'com.squareup.okhttp:okhttp:2.5.0'
    implementation 'org.altbeacon:android-beacon-library:2+'
}
```

For older gradle plugins use the *compile* directive like this:

```
dependencies {
    compile 'com.melrose.sojourn:sojourn:0.0.1@aar'
    compile 'com.android.support:appcompat-v7:27.1.1'
    //...
}
```

Import the library in the relevant java source files with:

```
import com.melrosecomputing.sojourn.export.*;
```

# API

All library methods are accessed via the static methods on the Sojourn class. e.g. `Sojourn.init(...)`

## Initialize Sojourn

```
void init(String applicationId, String serverUrl, Context context) 
```

The Sojourn library must be initialized within the `onCreate()` method of your app's Application subclass, as shown in the example below.

```
public class MainApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        Sojourn.init(<YOUR-APPLICATION-ID>, <YOUR-SERVER-URL>, this);
    }
}
```

Attempts to use any `Sojourn` method before initialization will fail.

## Enable Location 
To enable sojourn to start collecting location data you must explicitly enable it using the Sojourn class method enableLocation(). You must first ensure that the user has granted the ACCESS_FINE_LOCATION permission in your application or this setting will have no effect. This must be called after initialisation.

```
void enableLocation(boolean enabled)
```

A full example of checking permissions, requesting location permission and then enabling location in sojourn:

```
    private static final int MY_PERMISSION_REQUEST_READ_FINE_LOCATION = 111;
    
    private void requestEnableLocation() {
        // Check for permission
        if (ContextCompat.checkSelfPermission(this, android.Manifest.permission.ACCESS_FINE_LOCATION)
                != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(
                    this, // Activity
                    new String[]{android.Manifest.permission.ACCESS_FINE_LOCATION},
                    MY_PERMISSION_REQUEST_READ_FINE_LOCATION);
        } else {
            Sojourn.enableLocation(true);
        }
    }
    
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String permissions[], @NonNull int[] grantResults) {
        if (requestCode == MY_PERMISSION_REQUEST_READ_FINE_LOCATION) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                Sojourn.enableLocation(true);
            } else {
                Log.i("InboxActivity", "Fine location permission denied");
            }
        }

    }
```

## Custom Attribute
You can apply custom attributes to the device record.  Existing values will be overwritten.  This will allow for example more complex rules around sending messages based on gender or team preference etc.

Data types supported: String, Date, Number.

```
void applyCustomAttributes(HashMap<String, Object> attributes)
```

## Inbox
The inbox contains 0..* messages delivered by the platform. These may be generated by a user entering or exiting a geofence or beacon.

### Get Inbox Messages
Retrieves the messages for the specific device.

```
void getInbox(int skip, int limit, InboxResultCallback callback)

interface InboxResultCallback { 
    void handle(ArrayList<SojournMessage> messages, Error error); 
}
```

### Get Inbox Totals
Gets the total number of messages in the inbox and the total number of unread messages in the inbox.

```
void getInboxCount(InboxCountResultCallback callback)

interface InboxCountResultCallback { 
    void handle(int total, int unread); 
}
```

If an error occures then a value of -1 is passed to the callback.

### Mark all Messages as Read
Marks all messages in the inbox as read.

```
void markInboxAsRead(InboxBooleanResultCallback callback)

interface InboxBooleanResultCallback { 
    void handle(boolean succeeded, Error error); 
}
```

### Mark a Message as Read
Marks a specific message identified by it's objectId as read.

```
void markAsRead(String objectId, InboxBooleanResultCallback callback)

interface InboxBooleanResultCallback { 
    void handle(boolean succeeded, Error error); 
}
```

### Delete a Message
Deletes a specific message identified by it's objectId.

```
void deleteInboxMessage(String objectId, InboxBooleanResultCallback callback)
 
 interface InboxBooleanResultCallback { 
    void handle(boolean succeeded, Error error); 
 }
```

### Delete all Messages
Deletes all messages.

```
void deleteAllInboxMessages(InboxBooleanResultCallback callback)

interface InboxBooleanResultCallback { 
    void handle(boolean succeeded, Error error); 
}
```

## GDPR
Location data relating to individuals is very likely to be able to identify them. Hence it constitutes personal data and unless exemptions apply, the Data Protection Acts 1988 and 2003 (hereinafter called the ‘Data Protection Acts’) apply. It could also constitute sensitive data, and should therefore be handled carefully. Data controllers have a responsibility to minimise the amount of data collected, processed and retained because of risks posed by linked location data.

See here for more details:
[Guidance Note for Data Controllers on Location Data](https://www.dataprotection.ie/docs/Guidance-Note-for-Data-Controllers-on-Location-Data/1587.htm)
 
Informed consent is the most appropriate basis for processing personal location data in most cases.  This puts emphasis on the app developer to inform the user why they are collecting location data, what they are going to do with and how it will benefit the user.

To ensure Sojourn complies with GDPRs right to be forgotten we supply an API to allow users to mark their account as deleted.  This will record the users request to be forgotten and trigger an asynchronous process to delete the location data.

```
void deleteMe(DeleteMeBooleanResultCallback callback)

interface DeleteMeBooleanResultCallback { 
    void handle(boolean succeeded, Error error); 
}
```
