# Gradle License Plugin

[![License](https://img.shields.io/badge/license-apache%202.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Build Status](https://travis-ci.org/jaredsburrows/gradle-license-plugin.svg?branch=master)](https://travis-ci.org/jaredsburrows/gradle-license-plugin)
[![Coverage Status](https://coveralls.io/repos/github/jaredsburrows/gradle-license-plugin/badge.svg?branch=master)](https://coveralls.io/github/jaredsburrows/gradle-license-plugin?branch=master)
[![Twitter Follow](https://img.shields.io/twitter/follow/jaredsburrows.svg?style=social)](https://twitter.com/jaredsburrows)

This plugin provides a task to generate a HTML license report based on the 
configuration. (eg. `licenseDebugReport` for all debug dependencies in an Android project).

Applying this to an Android or Java project will generate a the license 
file(`open_source_licenses.html`) in the `<project>/build/reports/licenses/`.

Also, for Android projects the license HTML file will be copied to `<project>/src/main/assets/`.

## Download

Gradle:
```groovy
buildscript {
  repositories {
    jcenter()
  }

  dependencies {
    classpath "com.jaredsburrows:gradle-license-plugin:0.6.0"
  }
}

apply plugin: "com.android.application" // or "java"
apply plugin: "com.jaredsburrows.license"
```

Snapshot versions are available in the JFrog Artifactory repository: https://oss.jfrog.org/webapp/#/builds/gradle-license-plugin

## Tasks

- **`license${variant}Report`** for Android
- **`licenseReport`** for Java

Generates a HTML report of the all open source licenses. (eg. `licenseDebugReport` for all debug dependencies in an Android project).

Example `build.gradle`:

```groovy
dependencies {
  compile "com.android.support:design:25.1.0"
  compile "pl.droidsonroids.gif:android-gif-drawable:1.2.3"
}
```

HTML:
```html
<html>
  <head>
    <style>body{font-family:sans-serif;}pre{background-color:#eee;padding:1em;white-space:pre-wrap;}</style>
    <title>Open source licenses</title>
  </head>
  <body>
    <h3>Notice for libraries:</h3>
    <ul>
      <li>
        <a href='#-989311426'>Android GIF Drawable Library</a>
      </li>
      <li>
        <a href='#1288288048'>Design</a>
      </li>
    </ul>
    <a name='-989311426' />
    <h3>The MIT License</h3>
    <pre>The MIT License, http://opensource.org/licenses/MIT</pre>
    <a name='1288288048' />
    <h3>The Apache Software License</h3>
    <pre>The Apache Software License, http://www.apache.org/licenses/LICENSE-2.0.txt</pre>
  </body>
</html>
```

JSON:
```json
[
  {
    "project": "Android GIF Drawable Library",
    "developers": "Karol WrÃ³tniak",
    "url": "https://github.com/koral--/android-gif-drawable.git",
    "year": null,
    "version": "1.2.3",
    "license": "The MIT License",
    "license_url": "http://opensource.org/licenses/MIT"
  },
  {
    "project": "Design",
    "developers": null,
    "url": null,
    "year": null,
    "version": "25.1.0",
    "license": "The Apache Software License",
    "license_url": "http://www.apache.org/licenses/LICENSE-2.0.txt"
  }
]
```

## Usage

### Create an open source dialog
```java
public static class OpenSourceLicensesDialog extends DialogFragment {

  public OpenSourceLicensesDialog() {
  }

  @Override
  public Dialog onCreateDialog(Bundle savedInstanceState) {
    final WebView webView = new WebView(getActivity());
    webView.loadUrl("file:///android_asset/open_source_licenses.html");

    return new AlertDialog.Builder(getActivity())
      .setTitle("Open Source Licenses")
      .setView(webView)
      .setPositiveButton(R.string.ok, new DialogInterface.OnClickListener() {
        public void onClick(DialogInterface dialog, int which) {
          dialog.dismiss();
        }
      }
    )
    .create();
  }
}
```

### How to use it
```java
public static void showOpenSourceLicenses(Activity activity) {
  final FragmentManager fm = activity.getFragmentManager();
  final FragmentTransaction ft = fm.beginTransaction();
  final Fragment prev = fm.findFragmentByTag("dialog_licenses");
  if (prev != null) {
    ft.remove(prev);
  }
  ft.addToBackStack(null);

  new OpenSourceLicensesDialog().show(ft, "dialog_licenses");
}
```

Source: https://github.com/google/iosched/blob/2531cbdbe27e5795eb78bf47d27e8c1be494aad4/android/src/main/java/com/google/samples/apps/iosched/util/AboutUtils.java#L52

<img src="https://www.bignerdranch.com/assets/img/blog/2015/07/screenshot-gmail.png" />

Source: https://www.bignerdranch.com/blog/open-source-licenses-and-android/
