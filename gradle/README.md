```
// PROJECT-LEVEL
buildscript {
    repositories {
        google()
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath("com.android.tools.build:gradle:7.0.0")
        classpath("com.chaquo.python:gradle:12.2.0")
    }
}

// APP-LEVEL
plugins {
    id 'com.android.application'
    id 'com.chaquo.python'
}

android {
    compileSdkVersion 30
    defaultConfig {
        applicationId "com.example.chaquopyexample"
        minSdkVersion 21
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation "com.chaquo.python:chaquopy:12.2.0"
}
```
