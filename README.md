
Gradle Setting in Android Studio
=======

簡介如何在Android Studio裡設定Gradle


基本設定
---
```xml
apply plugin: 'com.android.application'

android {
    compileSdkVersion 22
    buildToolsVersion "22.0.1"

    defaultConfig {
        applicationId "com.crazyma.gradletest"
        minSdkVersion 19
        targetSdkVersion 22
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
```

上述是創建Android Studio Project時的基本設定。

singingConfigs
---
設定你的keystore。

```xml
signingConfigs {
        SproutSigning {
            keyAlias 'your_key_alias'
            keyPassword 'your_key_password'
            storeFile file('your_keystore_path')
            storePassword 'your_store_password'
        }
    }
```

實作上，有時為了不想要把 keystore 的密碼一併寫進 build.gradle，甚至是上傳的 Version Control System，所以可以把這些 info 寫進 **local.properties** 裡。可直接參考本 sample app。


buildTypes
---
最基本有兩種 build types：*debug* & *release*。你可以自訂你的build type。
build type裡可以設定 *Debugable*, *SigningConfig*, *Minify Enables*...等等，你可以進入Project Stucture > Build Types 瀏覽所有的設定。

以下是本 sample app 的設定：
```xml
buildTypes {
        debug {
            applicationIdSuffix ".debug"
        }

        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
```
>**Explanation**：
>1. 如果使用 *debug build type*，會自動在 application id 後面添加 .debug。以本 sample app 為例，application id 就會變成 **com.sprout.gradletest.debug**
>2. 如果使用 *release build type*，會啟用 **proguard**，並且參照 proguard-rules.pro的規則
>3. 小提醒，debug build type 無法啟用 proguard

productFlavors
---
可以在Flavors設定的項目如下：
1. Min SDK Version
2. Target SDK Version
3. Application ID
4. Version Name & Version Code
5. Signing Config
6. ...

詳情可以至 Project Structure > Flavors 瀏覽所有的設定。

以下是本 sample app 的設定
```xml
productFlavors {
        demo {
            applicationId 'com.sprout.gradletest.demo'
            versionName "1.0-demo"
        }
        pub {
            applicationId "com.sprout.gradletest.pub"
            versionName "1.0-pub"
        }
    }
```
>**Explanation**：
>1. 這裏設定兩個 flavors：*demo* & *pub*
>2. 兩個 flavor 有各自的 versionName & Application ID
>3. 使用時，可以從Android Studio 側邊選單 > Build Variants 選擇。此時，依照上述的 build type 設定，你會有四個選項：demoDebug, demoRelease, pubDebug, pubRelease


Work with build variants
---
你可以依據不同的**flavor**，設定不同的 code, layout or resources。
詳細實作過程可參照[官網][1]。

本sample有實作該效果，可直接參考 sample code。
>注意事項：
>1. create new folder時，需要建立package name，記得要跟原本的package name一樣(反而是不用跟 application id 一樣)
>2. 如果有兩個flavor，當你的build Variant設定為first flavor時，second flavor不會被處理，所以會被標示呈錯誤，不要太緊張。
>3. 如果在 AndroidManifest 宣告中使用自訂的package path(例如使用GCM時會遇到)，記得要改成@{applicationId}。[參考][2]


----------


License
-------
	Copyright 2015 David Ma

	Licensed under the Apache License, Version 2.0 (the "License");
	you may not use this file except in compliance with the License.
	You may obtain a copy of the License at

	   http://www.apache.org/licenses/LICENSE-2.0

	Unless required by applicable law or agreed to in writing, software
	distributed under the License is distributed on an "AS IS" BASIS,
	WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	See the License for the specific language governing permissions and
	limitations under the License.


[1]: https://developer.android.com/tools/building/configuring-gradle.html#workBuildVariants
[2]: http://stackoverflow.com/questions/27154990/android-l-permission-conflict-between-release-and-debug-apks
