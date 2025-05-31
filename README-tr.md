# 📱 Flutter ile Dinamik Uygulama İkonu Kullanımı (android_dynamic_icon)

Bu doküman, Flutter projesine android_dynamic_icon eklentisinin manuel olarak nasıl entegre edileceğini ve Android uygulama ikonunun nasıl dinamik olarak değiştirileceğini adım adım açıklar.

## 🔧 1. Eklentiyi Projeye Ekle

pubspec.yaml dosyanı aç ve dependencies kısmına aşağıdaki satırı ekle:

```dependencies:
  android_dynamic_icon:
    git:
      url: https://github.com/bilalbaz1/android_dynamic_icon.git
      ref: 37810be
```

Terminalde çalıştır:

```flutter pub get```

## 🖼 2. İkonları drawable klasörüne ekle

Tüm ikon dosyalarını (örneğin: iconone.png, icontwo.png) şu klasöre yerleştir:

`android/app/src/main/res/drawable`

İkonlar .png formatında olmalı.

## 📝 3. AndroidManifest.xml Güncellemesi

`android/app/src/main/AndroidManifest.xml` dosyasını aç ve <application> bloğunu aşağıdaki şekilde güncelle:

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

    <!-- Alternatif ikonlar için activity-alias tanımları -->
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
</application>
````

## 🧾 4. `IconOne.kt` ve `IconTwo.kt` Dosyalarını Oluştur

`android/app/src/main/kotlin/com/example/uygulamaadi/` klasörüne git. (Kendi package adına göre klasör yapısı değişebilir.)

Aşağıdaki iki dosyayı oluştur:
`IconOne.kt`

````
package com.example.uygulamaadi

import io.flutter.embedding.android.FlutterActivity

class IconOne : FlutterActivity() {
}
````

`IconTwo.kt`

```
package com.example.uygulamaadi

import io.flutter.embedding.android.FlutterActivity

class IconTwo : FlutterActivity() {
}
```

‼️ ‼️ ‼️ ‼️ package adını kendi projenin package adı ile değiştir. ‼️ ‼️ ‼️


#
#

## ✅ 5. Flutter Tarafında Kullanım

İkonu değiştirmek için Flutter kodu:

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

Varsayılan ikona geri dönmek için:

```
await AndroidDynamicIcon.changeIcon(classNames: ['MainActivity', '']);
````

🔁 Son

Artık uygulaman, kullanıcıya veya belirli olaylara göre farklı ikonlar gösterebilir. Örneğin kampanyalarda, yılbaşında veya doğum günlerinde özel ikon kullanabilirsin.

#
#
# Örnek Kod:
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