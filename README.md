### Languages: &nbsp; [Türkçe](https://github.com/bilalbaz1/android_dynamic_icon/blob/main/README-tr.md) &nbsp; &nbsp;
&nbsp;

# 📱 Using Dynamic Application Icon with Flutter (android_dynamic_icon)

This document explains step by step how to manually integrate the android_dynamic_icon plugin into a Flutter project and how to dynamically change the Android application icon.

## 🔧 1. Add the Plugin to the Project

Open your pubspec.yaml file and add the following line to the dependencies section:

```dependencies:
  android_dynamic_icon:
    git:
      url: https://github.com/bilalbaz1/android_dynamic_icon.git
      ref: 37810be
```

Run in the terminal:

```flutter pub get```

## 🖼 2. Add the icons to the drawable folder

Place all icon files (for example: iconone.png, icontwo.png) in the following folder:

`android/app/src/main/res/drawable`

The icons must be in .png format.

## 📝 3. Update the AndroidManifest.xml

`android/app/src/main/AndroidManifest.xml` file open and update the <application> block as follows::

```
<application
    android:name="io.flutter.app.FlutterApplication"
    android:label="your_app_name"
    android:icon="@drawable/iconone"
    android:roundIcon="@drawable/iconone"
    android:theme="@style/LaunchTheme"
    android:requestLegacyExternalStorage="true">

    <activity
        android:name=".MainActivity"
        android:exported="true"
        android:launchMode="singleTop"
        android:theme="@style/LaunchTheme"
        android:configChanges="orientation|keyboardHidden|keyboard|screenSize|smallestScreenSize|locale|layoutDirection|fontScale|screenLayout|density|uiMode"
        android:hardwareAccelerated="true"
        android:windowSoftInputMode="adjustResize">
        <meta-data
            android:name="io.flutter.embedding.android.NormalTheme"
            android:resource="@style/NormalTheme"/>
        <intent-filter>
            <action android:name="android.intent.action.MAIN"/>
            <category android:name="android.intent.category.LAUNCHER"/>
        </intent-filter>
    </activity>

    <!-- activity-alias start -->
    <activity-alias
        android:label="app"
        android:icon="@drawable/iconone"
        android:name=".IconOne"
        android:enabled="false"
        android:exported="true"
        android:targetActivity=".MainActivity">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity-alias>

    <activity-alias
        android:label="app"
        android:icon="@drawable/icontwo"
        android:name=".IconTwo"
        android:enabled="false"
        android:exported="true"
        android:targetActivity=".MainActivity">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity-alias>
    <!-- activity-alias end -->
    
</application>
````

## 🧾 4. `IconOne.kt` and `IconTwo.kt` create file.

Go to the `android/app/src/main/kotlin/com/example/appname/` folder. (The folder structure may vary according to your package name.)

Create the following two files:

`IconOne.kt`

````
package com.example.appname

import io.flutter.embedding.android.FlutterActivity

class IconOne : FlutterActivity() {
}
````

`IconTwo.kt`

```
package com.example.appname

import io.flutter.embedding.android.FlutterActivity

class IconTwo : FlutterActivity() {
}
```

‼️ ‼️ ‼️ ‼️ Replace the package name with your project's package name. ‼️ ‼️ ‼️


#
#

## ✅ 5. Usage on the Flutter Side

Flutter code to change the icon:

```
import 'package:android_dynamic_icon/android_dynamic_icon.dart';

final _androidDynamicIconPlugin = AndroidDynamicIcon();

@override
void initState() {
AndroidDynamicIcon.initialize(classNames: ['MainActivity', 'IconOne', 'IconTwo']);
super.initState();
}

void changeFunc() async {
    await _androidDynamicIconPlugin.changeIcon(
        classNames: ['IconOne', ''],
    );
}
````

To revert to the default icon:

```
await AndroidDynamicIcon.changeIcon(classNames: ['MainActivity', '']);
````

🔁 End

Your application can now display different icons based on the user or specific events. For example, you can use special icons during campaigns, New Year's, or birthdays.

#
#
# Example Code:
```
import 'package:android_dynamic_icon/android_dynamic_icon.dart';
import 'package:flutter/material.dart';

class ChangeIconPage extends StatefulWidget {
  const ChangeIconPage({super.key});

  @override
  State<ChangeIconPage> createState() => _ChangeIconPageState();
}

class _ChangeIconPageState extends State<ChangeIconPage> {

  final _androidDynamicIconPlugin = AndroidDynamicIcon();

  @override
  void initState() {
    AndroidDynamicIcon.initialize(
        classNames: ['MainActivity', 'IconOne', 'IconTwo']);
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(

      body: ListView(
        children: [
          OutlinedButton.icon(
            icon: const Icon(Icons.ac_unit),
            label: Text("IconOne"),
            onPressed: () async {
              try {
                await _androidDynamicIconPlugin.changeIcon(
                  classNames: ['IconOne', ''],
                );
              } catch (_) {
                if (context.mounted) {
                  ScaffoldMessenger.of(context).hideCurrentSnackBar();
                  ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
                    duration: Duration(seconds: 3),
                    content: Text("Failed to change app icon"),
                  ));
                }
              }
            },
          ),
          Text("--------"),
          OutlinedButton.icon(
            icon: const Icon(Icons.ac_unit),
            label: Text("IconTwo"),
            onPressed: () async {
              try {
                await _androidDynamicIconPlugin.changeIcon(
                  classNames: ['IconTwo', ''],
                );
              } catch (_) {
                if (context.mounted) {
                  ScaffoldMessenger.of(context).hideCurrentSnackBar();
                  ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
                    duration: Duration(seconds: 3),
                    content: Text("erorrrr !!!! Failed to change app icon"),
                  ));
                }
              }
            },
          ),
          Text("--------"),
          OutlinedButton.icon(
            icon: const Icon(Icons.ac_unit),
            label: Text("default"),
            onPressed: () async {
              try {
                await _androidDynamicIconPlugin.changeIcon(
                  classNames: ['MainActivity', ''],
                );
              } catch (_) {
                if (context.mounted) {
                  ScaffoldMessenger.of(context).hideCurrentSnackBar();
                  ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
                    duration: Duration(seconds: 3),
                    content: Text("erorrrr !!!! Failed to change app icon"),
                  ));
                }
              }
            },
          ),
        ],
      ),
    );
  }
}
```