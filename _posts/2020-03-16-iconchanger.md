
      ---
layout: post
title: IconChanger
date: 2020-03-16 17:25:20 +0300
description: IconChanger
img: IconChanger.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [IconChanger]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java


//EXAMPLE
//Create MoreBlock with "String, and List String";
//setAppIcon(_active, _disable);

//To use block
//ActiveName is default icon like this to set it:
//com.ebook.sketchware.MainActivity-default

//Disable Name Are list string to disable it example:
//com.ebook.sketchware.MainActivity-java
//com.ebook.sketchware.MainActivity-black

//And Add activity-alias on Manifest like this:
//true is default
//Dont forget to remove tag intent-filter from default activity

        <activity-alias
            android:enabled="true" 
            android:icon="@drawable/app_icon" 
            android:label="Java Library" 
            android:name="com.ebook.sketchware.MainActivity-default" 
            android:targetActivity=".MainActivity">
			<intent-filter>
				<action android:name="android.intent.action.MAIN" />
				<category android:name="android.intent.category.LAUNCHER" />
			</intent-filter>
		</activity-alias>
		<activity-alias 
            android:enabled="false" 
            android:icon="@drawable/icon_java" 
            android:label="Java Library" 
            android:name="com.ebook.sketchware.MainActivity-java" 
            android:targetActivity=".MainActivity">
			<intent-filter>
				<action android:name="android.intent.action.MAIN" />
				<category android:name="android.intent.category.LAUNCHER" />
			</intent-filter>
		</activity-alias>
		<activity-alias 
            android:enabled="false" 
            android:icon="@drawable/icon_black" 
            android:label="Java Library" 
            android:name="com.ebook.sketchware.MainActivity-black" 
            android:targetActivity=".MainActivity">
			<intent-filter>
				<action android:name="android.intent.action.MAIN" />
				<category android:name="android.intent.category.LAUNCHER" />
			</intent-filter>
		</activity-alias>
		



//Library By Aan Gabriel Gymkhana

public void setAppIcon(String activeName, List<String> disableName) {
new AppIconNameChanger.Builder(MainActivity.this)
.activeName(activeName) // String
.disableNames(disableName) // List<String>
.packageName(getApplicationContext().getPackageName())
.build()
.setNow();
}


public static class AppIconNameChanger {

    private Activity activity;
    List<String> disableNames;
    String activeName;
    String packageName;

    public AppIconNameChanger(Builder builder) {

        this.disableNames = builder.disableNames;
        this.activity = builder.activity;
        this.activeName = builder.activeName;
        this.packageName = builder.packageName;

    }

    public static class Builder {

        private Activity activity;
        List<String> disableNames;
        String activeName;
        String packageName;

        public Builder(Activity activity) {
            this.activity = activity;
        }

        public Builder disableNames(List<String> disableNamesl) {
            this.disableNames = disableNamesl;
            return this;
        }

        public Builder activeName(String activeName) {
            this.activeName = activeName;
            return this;
        }

        public Builder packageName(String packageName) {
            this.packageName = packageName;
            return this;
        }

        public AppIconNameChanger build() {
            return new AppIconNameChanger(this);
        }

    }

    public void setNow() {

        activity.getPackageManager().setComponentEnabledSetting(
                new android.content.ComponentName(packageName, activeName),
                android.content.pm.PackageManager.COMPONENT_ENABLED_STATE_ENABLED, android.content.pm.PackageManager.DONT_KILL_APP);

        for (int i = 0; i < disableNames.size(); i++) {
            try {
                activity.getPackageManager().setComponentEnabledSetting(
                        new android.content.ComponentName(packageName, disableNames.get(i)),
                        android.content.pm.PackageManager.COMPONENT_ENABLED_STATE_DISABLED, android.content.pm.PackageManager.DONT_KILL_APP);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

```
      