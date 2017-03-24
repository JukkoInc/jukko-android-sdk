# Jukko SDK documentation

Welcome to the Jukko developer SDK documentation. Follow our step-by-step instructions to
integrate Jukko and start monetizing while converting your app into a catalyst for social
impact today!

If you have any questions during the integration process, you can reach us at [devs@jukko.com](mailto:devs@jukko.com)
and we'll get back to you ASAP. Thanks for joining our movement to create a better world and
a better way of doing business.

1. [Integration](#integration)
2. [Usage](#usage)
    * [Initialization](#usage)
    * [Ad showing](#ad-showing)
    * [Frequnecy capping](#frequnecy-capping)
    * [Console logging](#console-logging)
3. [Google Play Services](#google-play-services)
4. [Requirements](#requirements)

### Integration

1. Add following lines to your root project `build.gradle`:

        :::gradle
        allprojects {
            repositories {
                ....
                maven { url "https://dl.jukko.com/android-sdk" }
            }
        }


2. In your application `build.gradle` add following line to the dependencies:

        :::gradle
        dependencies {
            compile 'com.jukko.sdk:jukko-sdk:$version'
        }

### Usage

##### Initialization
Initialization has to be done before Jukko can be customized and launched. The method will
need the API key and context. The API key can be generated in the dashboard after registration.
You can register on [Jukko website](https://jukko.com).

```java
    JukkoSdk.getInstance().init(context, "API_KEY");
```


##### Ad showing

You can show an ad by calling `showAd()` method:

```java
        JukkoSdk.getInstance().showAd(new JukkoSdkInterface.AdCallback() {
            @Override
            public void onClosed(JukkoSdkInterface.AdClosedEvent event) {
                //handle onClosed events here
            }
        });
```

When ads activity is finished, `onClosed()` callback will be called on main thread. It will following information:

1. `reason`: reason why ad was closed. Possible variants are:
    * `CLOSED_BY_USER`: Ad view was closed by user interactions
    * `TIMEOUT`: Api servers were unresponsive
    * `NETWORK_CONNECTIVITY`: Network connectivity problems
    * `FREQUENCY_CAPPING`: `showAd()` called before frequency capping timeout ended
    * `ERROR`: Unspecified error. Look at the `message` field for description
2. `message`: string containing extended description of reason
3. `events`: list of events that happened with Ad activity. May be null. Each event contains:
    * `timestamp` of the event (uses current timezone)
    * `adEvent` type of event. Possible variants:
        * `LAUNCH`: Ad activity opened
        * `CLOSE`: Ad activity closed
        * `INTRO_SHOWN`: NPO campaign intro was shown to user
        * `PROGRESS_SHOWN`: NPO campaign progress was shown to user
        * `OUTRO_SHOWN`: NPO campaign outro was shown to user
        * `AD_SHOWN`: Ad was shown to user
        * `AD_URL_OPENED`: user clicked on url, that opened in external browser


##### Frequency capping

Sdk allows developer to set frequency capping for ads. It counts time since last ad was closed
and ignores `showAd()` calls in frequency capping period. Frequency capping can be changed
using:

```java
    JukkoSdk.getInstance().setAdsFrequency(timeInSeconds);
```


##### Console logging

By default, Jukko SDK logs only important messages, like unrecoverable error reasons.

You can enable debug logging by calling

```java
    JukkoSdk.getInstance().setDebugMode(true);
```
Log messages will contain `Jukko SDK` tag.


### Google Play Services

If user have Google Play services on their devices, SDK will automatically use its
Advertising ID (GAID) and Limit Ad Tracking information.

For more information about Google Advertising ID visit [this link](https://play.google.com/about/monetization-ads/ads/ad-id/)

### Requirements

Jukko SDK support devices starting with Android 4.0 (API level 14).

However, SDK won't do anything for Android prior to 4.4 (API level < 19) except log messages.