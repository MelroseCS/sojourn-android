## Android Sojourn SDK

### Android Studio Integration

Add this repository to the app-level build.gradle file:

```
allprojects {
    repositories {
        maven{ 
            url "https://raw.githubusercontent.com/MelroseCS/sojourn-android/stable/" 
        }
    }
}
```

Add the library with version to your dependencies:

```
dependencies {
    compile 'com.melrose.sojourn:sojourn:0.0.1@aar'
    compile 'com.android.support:appcompat-v7:27.1.1'
    compile 'com.android.support:support-v4:27.1.1'
    compile 'com.google.android.gms:play-services-location:15.0.1'
    compile 'com.google.android.gms:play-services-maps:15.0.1'
    compile 'com.parse:parse-android:1.17.3'
    compile 'com.parse:parse-fcm-android:1.17.3'
    compile 'com.squareup.okhttp:okhttp:2.5.0'
    compile 'org.altbeacon:android-beacon-library:2+'
}
```

Import the library in java files with:

```
import com.melrosecomputing.sojourn.export.*;
```
