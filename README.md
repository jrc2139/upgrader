# Upgrader

Flutter package for prompting users to upgrade when there is a newer version of the app in the store.

[![Build Status](https://travis-ci.com/larryaasen/upgrader.svg?branch=master)](https://app.travis-ci.com/github/larryaasen/upgrader)
[![Codemagic](https://api.codemagic.io/apps/5ffb7888eb8402dcd1928753/flutter-package/status_badge.svg)](https://codemagic.io/apps/5ffb7888eb8402dcd1928753/flutter-package/latest_build)
[![codecov](https://codecov.io/gh/larryaasen/upgrader/branch/master/graph/badge.svg)](https://app.codecov.io/gh/larryaasen/upgrader)
[![pub package](https://img.shields.io/pub/v/upgrader.svg)](https://pub.dartlang.org/packages/upgrader)
[![GitHub Stars](https://img.shields.io/github/stars/larryaasen/upgrader.svg)](https://github.com/larryaasen/upgrader/stargazers)
<a href="https://www.buymeacoffee.com/larryaasen">
  <img alt="Buy me a coffee" src="https://img.shields.io/badge/Donate-Buy%20Me%20A%20Coffee-yellow.svg">
</a>
[![Twitter](https://img.shields.io/twitter/url/https/twitter.com/upgraderpackage.svg?style=social&label=Follow%20%40upgraderpackage)](https://twitter.com/upgraderpackage)

When a newer app version is availabe in the app store, a simple alert prompt or card is
displayed. With today's modern app stores, there is little need to persuade users to upgrade
because most are already using the auto upgrade feature. However, there may be times when
an app needs to be updated more quickly than usual, and nagging a user to upgrade will entice
the upgrade sooner. Also, with Flutter supporting more than just Android and iOS platforms in the
future, it will become more likely that users on other app stores need to be nagged about
upgrading.

### UI
The UI comes in two flavors: alert or card. The [UpgradeAlert](#alert-example) class is used to display the
popup alert prompt, and the [UpgradeCard](#card-example) class is used to display the inline material design card.

### Localization
The text displayed in the `upgrader` package is localized in [many languages](#language-localization), and supports customization.

### Release Notes
The release notes are displayed by default when a new version is available. On Android
the release notes are taken from the the WHAT'S NEW section on Google Play when
available, otherwise the main app description is used.
On iOS the release notes are taken from the App Store What's New section.
For [appcast](#appcast)), the release notes are taken from the description field.

### Minimum App Version
The `upgrader` package can use a forced upgrade version (minimum app version)
simply by adding that 
version number to the description field in the app stores. Use this format:
```
[:mav: 1.2.3]
```

Using that text says that the minimum app version is 1.2.3 and that earlier
versions of this app will be forced to update to the current version.

After the app containing this text has been submitted for review, approved, and
released on the app store, the version number will be visible to the upgrader
package. When the minimum app version is updated in the future, all previously
installed apps with this package (version 3.9.0+) will recognize and honor
that vaule.

#### Android
Add this text to the bottom of the Full description field in the Google Play
Console under the Main store listing.

#### iOS
Add this text to the bottom of the Description field in AppStoreConnet in the
Desctiption field.

## Alert Example

Just wrap your body widget in the `UpgradeAlert` widget, and it will handle the rest.
```dart
import 'package:flutter/material.dart';
import 'package:upgrader/upgrader.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  MyApp({
    Key key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Upgrader Example',
      home: Scaffold(
          appBar: AppBar(
            title: Text('Upgrader Example'),
          ),
          body: UpgradeAlert(
            child: Center(child: Text('Checking...')),
          )
      ),
    );
  }
}
```

## Screenshot of alert

![image](screenshots/example1.png)


## Cupertino Alert Example

You can also display a Cupertino style dialog by using the `dialogStyle` parameter.
```dart
          body: UpgradeAlert(
            dialogStyle: UpgradeDialogStyle.cupertino,
            child: Center(child: Text('Checking...')),
          )
```

## Screenshot of Cupertino alert

![image](screenshots/example-cupertino.png)

## Card Example

Just return an `UpgradeCard` widget in your build method and a material design card will be displayed
when an update is detected. The widget will have width and height of 0.0 when no update is detected.
```dart
return Container(
        margin: EdgeInsets.fromLTRB(12.0, 0.0, 12.0, 0.0),
        child: UpgradeCard());
```

## Screenshot of card

![image](screenshots/example2.png)

## Customization

The UpgradeAlert widget can be customized by setting parameters in the constructor of the
UpgradeAlert widget.

* appcastConfig: the appcast configuration, defaults to ```null```
* client: an HTTP Client that can be replaced for mock testing, defaults to ```null```
* durationUntilAlertAgain: duration until alerting user again, which defaults to ```3 days```
* debugDisplayAlways: always force the upgrade to be available, defaults to ```false```
* debugDisplayOnce: display the upgrade at least once once, defaults to ```false```
* debugLogging: display logging statements, which defaults to ```false```
* messages: optional localized messages used for display in `upgrader`
* onIgnore: called when the ignore button is tapped, defaults to ```null```
* onLater: called when the later button is tapped, defaults to ```null```
* onUpdate: called when the update button is tapped, defaults to ```null```
* shouldPopScope: called when the back button is tapped, defaults to ```null```
* showIgnore: hide or show Ignore button, which defaults to ```true```
* showLater: hide or show Later button, which defaults to ```true```
* showReleaseNotes: hide or show release notes, which defaults to ```true```
* canDismissDialog: can alert dialog be dismissed on tap outside of the alert dialog, which defaults to ```false``` (not used by UpgradeCard)
* countryCode: the country code that will override the system locale, which defaults to ```null``` (iOS only)
* minAppVersion: the minimum app version supported by this app. Earlier versions of this app will be forced to update to the current version. Defaults to ```null```.
* dialogStyle: the upgrade dialog style, either ```material``` or ```cupertino```, defaults to ```material```, used only by UpgradeAlert, works on Android and iOS.

## Android Back Button

When using the ```UpgradeAlert``` widget, the Android back button will not
dismiss the alert dialog by default. To allow the back button to dismiss the
dialog, use ```shouldPopScope``` and return true like this:
```
UpgradeAlert(
  shouldPopScope: () => true,
);
```

## iOS Country Code

When your app is not in the iOS `US` App Store, which is the default, you must use
the `countryCode` parameter mentioned above. The `upgrader` package does not know
which country app store to use because it is not provided by Apple. It assumes
the app is in the `US` App Store.

## Limitations
These widgets work on both Android and iOS. When running on Android the Google
Play Store will provide the latest app version.
 When running on iOS the App Store will provide the
latest app version. In all cases, the widget will display the prompt at the
appropriate times.

On Android, the version number is often not available from the Google Play
Store, such as with the
[Google Maps](https://play.google.com/store/apps/details?id=com.google.android.apps.maps)
app. In this case, the version is listed as `Varies with device`. That is not a
valid version for `upgrader` and cannot be used. The `upgrader` widget will not be
displayed in this case.

There is an [appcast](#appcast) that can be used to remotely configure the
latest app version. See [appcast](#appcast) below for more details.

## Appcast

The class [Appcast](lib/src/appcast.dart), in this Flutter package, is used by the `upgrader` widgets
to download app details from an appcast,
based on the [Sparkle](https://sparkle-project.org/) framework by Andy Matuschak.
You can read the Sparkle documentation here:
https://sparkle-project.org/documentation/publishing/.

An appcast is an RSS feed with one channel that has a collection of items that each describe
one app version. The appcast will decscribe each app version and will provide the latest app
version to `upgrader` that indicates when an upgrade should be recommended.

The appcast must be hosted on a server that can be reached by everyone from the app. The appcast
XML file can be autogenerated during the release process, or just manually updated after a release
is available on the app store.

The Appcast class can be used stand alone or as part of `upgrader`.

### Appcast Example
This is an Appcast example for Android.
```dart
@override
Widget build(BuildContext context) {
  // On Android, setup the Appcast.
  // On iOS, the default behavior will be to use the App Store version.
  final appcastURL =
      'https://raw.githubusercontent.com/larryaasen/upgrader/master/test/testappcast.xml';
  final cfg = AppcastConfiguration(url: appcastURL, supportedOS: ['android']);

  return MaterialApp(
    title: 'Upgrader Example',
    home: Scaffold(
        appBar: AppBar(
          title: Text('Upgrader Example'),
        ),
        body: UpgradeAlert(
          appcastConfig: cfg,
          child: Center(child: Text('Checking...')),
        )),
  );
}
```

### Appcast Sample File
```xml
<?xml version="1.0" encoding="utf-8"?>
<rss version="2.0" xmlns:sparkle="http://www.andymatuschak.org/xml-namespaces/sparkle">
    <channel>
        <title>Debt Now App - Appcast</title>
        <item>
            <title>Version 1.15.0</title>
            <description>Minor updates and improvements.</description>
            <pubDate>Sun, 30 Dec 2018 12:00:00 +0000</pubDate>
            <enclosure url="https://play.google.com/store/apps/details?id=com.moonwink.treasury" sparkle:version="1.15.0" sparkle:os="android" />
        </item>
    </channel>
</rss>
```

### Appcast Class
```dart
final appcast = Appcast();
final items = await appcast.parseAppcastItemsFromUri('https://raw.githubusercontent.com/larryaasen/upgrader/master/test/testappcast.xml');
final bestItem = appcast.bestItem();
```

## Customizing the display

The strings displayed in `upgrader` can be customzied by extending the `UpgraderMessages` class
to provide custom values.

As an example, to replace the Ignore button with a custom value, first create a new
class that extends UpgraderMessages, and override the buttonTitleIgnore function. Next,
when calling UpgradeAlert (or UpgradeCard), add the paramter messages with an instance
of your extended class. Here is an example:

```dart
class MyUpgraderMessages extends UpgraderMessages {
  @override
  String get buttonTitleIgnore => 'My Ignore';
}

UpgradeAlert(messages: MyUpgraderMessages());
```

## Language localization

The strings displayed in `upgrader` are already localized in various languages. New languages will be
supported in the future with minor updates.

Languages supported:
* English ('en')
* Arabic ('ar')
* Bengali ('bn')
* Filipino ('fil')
* French ('fr')
* German ('de')
* Greek ('el')
* Hungarian ('hu')
* Indonesian ('id')
* Italian ('it')
* Kazakh ('kk')
* Korean ('ko')
* Lithuanian ('lt')
* Norwegian ('nb')
* Persian ('fa')
* Polish ('pl')
* Portuguese ('pt')
* Russian ('ru')
* Spanish ('es')
* Tamil ('ta')
* Turkish ('tr')
* Ukrainian ('uk')
* Vietnamese ('vi')

The `upgrader` package can be supplied with additional languages in your code by extending the `UpgraderMessages` class
to provide custom values.

As an example, to add the Spanish (es) language (which is already provided), first create a new
class that extends UpgraderMessages, and override the message function. Next, add a string for
each of the messages. Finally, when calling UpgradeAlert (or UpgradeCard), add the paramter messages with an instance
of your extended class. Here is an example:

```dart
class MySpanishMessages extends UpgraderMessages {
  /// Override the message function to provide custom language localization.
  @override
  String message(UpgraderMessage messageKey) {
    if (languageCode == 'es') {
      switch (messageKey) {
        case UpgraderMessage.body:
          return 'es A new version of {{appName}} is available!';
        case UpgraderMessage.buttonTitleIgnore:
          return 'es Ignore';
        case UpgraderMessage.buttonTitleLater:
          return 'es Later';
        case UpgraderMessage.buttonTitleUpdate:
          return 'es Update Now';
        case UpgraderMessage.prompt:
          return 'es Want to update?';
        case UpgraderMessage.title:
          return 'es Update App?';
      }
    }
    // Messages that are not provided above can still use the default values.
    return super.message(messageKey);
  }
}

UpgradeAlert(messages: MySpanishMessages());
```

You can even force the `upgrader` package to use a specific language, instead of the
system language on the device. Just pass the language code to an instance of
UpgraderMessages when displaying the alert or card. Here is an example:

```dart
UpgradeAlert(messages: UpgraderMessages(code: 'es'));
```

## Semantic Versioning

The `upgrader` package uses the [version](https://pub.dev/packages/version) package that
is in compliance with the Semantic Versioning spec at http://semver.org/.


## iTunes Search API

There is a class in this Flutter package used by the `upgrader` widgets to download app details
from the
[iTunes Search API](https://affiliate.itunes.apple.com/resources/documentation/itunes-store-web-service-search-api).
The class ITunesSearchAPI can be used standalone to query iTunes for app details.

### ITunesSearchAPI Example
```dart
final iTunes = ITunesSearchAPI();
final resultsFuture = iTunes.lookupByBundleId('com.google.Maps');
resultsFuture.then((results) {
    print('results: $results');
});
```

### Results
[![image](screenshots/results.png)](screenshots/results.png)


### Command Line App - Android

There is a command line app used to display the results from Google Play Store. The code is located in
bin/playstore_lookup.dart, and can be run from the command line like this:
```
$ dart playstore_lookup.dart id=com.google.android.apps.mapslite
```
Results:
```
playstore_lookup releaseNotes: • Support plus.codes URLs• Bug fixes
playstore_lookup version: 152.0.0
...
```

### Command Line App - iOS
There is a command line app used to display the results from iTunes Search. The code is located in
bin/itunes_lookup.dart, and can be run from the command line like this:
```
$ dart itunes_lookup.dart bundleid=com.google.Maps
```
Results:
```
upgrader: download: https://itunes.apple.com/lookup?bundleId=com.google.Maps
upgrader: response statusCode: 200
itunes_lookup bundleId: com.google.Maps
itunes_lookup releaseNotes: Thanks for using Google Maps!
itunes_lookup trackViewUrl: https://apps.apple.com/us/app/google-maps-transit-food/id585027354?uo=4
itunes_lookup version: 5.58
itunes_lookup all results:
{resultCount: 1, results:
...
```

## Reporting Issues

Please submit issue reports [here on GitHub](https://github.com/larryaasen/upgrader/issues/new/choose).
To better assist in analyzing issues, please include all of the `upgrader` log, which should look something like this:
```
flutter: upgrader: languageCode: en
flutter: upgrader: build UpgradeAlert
flutter: upgrader: default operatingSystem: ios 11.4
flutter: upgrader: operatingSystem: ios
flutter: upgrader: platform: TargetPlatform.iOS
flutter: upgrader: package info packageName: com.google.Maps
flutter: upgrader: package info appName: Upgrader
flutter: upgrader: package info version: 1.0.0
flutter: upgrader: countryCode: US
flutter: upgrader: blocked: false
flutter: upgrader: debugDisplayAlways: false
flutter: upgrader: debugDisplayOnce: false
flutter: upgrader: hasAlerted: false
flutter: upgrader: appStoreVersion: 5.81
flutter: upgrader: installedVersion: 1.0.0
flutter: upgrader: minAppVersion: null
flutter: upgrader: isUpdateAvailable: true
flutter: upgrader: shouldDisplayUpgrade: true
flutter: upgrader: shouldDisplayReleaseNotes: true
flutter: upgrader: showDialog title: Update App?
flutter: upgrader: showDialog message: A new version of Upgrader is available! Version 5.81 is now available-you have 1.0.0.
flutter: upgrader: showDialog releaseNotes: Thanks for using Google Maps! This release brings bug fixes that improve our product to help you discover new places and navigate to them.
```
Also, please include the upgrader version number from the pubspec.lock file, which should look something like this:
```
  upgrader:
    dependency: "direct main"
    description:
      path: ".."
      relative: true
    source: path
    version: "3.6.0"
```


## Contributing
All [comments](https://github.com/larryaasen/upgrader/issues) and [pull requests](https://github.com/larryaasen/upgrader/pulls) are welcome.

## Donations / Sponsor

Please sponsor or donate to the creator of `upgrader` on [Flattr](https://flattr.com/@larryaasen) or [Patreon](https://www.patreon.com/larryaasen).
