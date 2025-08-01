# Voice Location Android App

An Android application built in Java that listens for the voice command "bring my table", fetches the user's current GPS location, displays it on Google Maps, and sends the coordinates to an IFTTT webhook.

## Features

- **Voice Recognition**: Uses Android's SpeechRecognizer to listen for "bring my table" command
- **GPS Location**: Fetches current location using FusedLocationProviderClient
- **Google Maps Integration**: Displays location on an interactive Google Map with markers
- **IFTTT Integration**: Sends latitude and longitude to IFTTT webhook via GET request
- **Real-time UI Updates**: Shows recognized commands and location details on screen
- **Permission Handling**: Properly requests and handles RECORD_AUDIO, ACCESS_FINE_LOCATION, and INTERNET permissions

## Prerequisites

- Android Studio (latest version recommended)
- Android SDK API Level 24 or higher
- Google Maps API Key
- IFTTT Webhook URL (optional)

## Setup Instructions

### 1. Clone/Download the Project
Download or clone this project to your local machine.

### 2. Open in Android Studio
1. Open Android Studio
2. Select "Open an existing Android Studio project"
3. Navigate to the `android-voice-location-app` folder and select it

### 3. Configure Google Maps API Key

#### Get Google Maps API Key:
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the "Maps SDK for Android" API
4. Go to "Credentials" and create a new API key
5. Restrict the API key to Android apps (recommended for security)

#### Add API Key to the App:
1. Open `app/src/main/AndroidManifest.xml`
2. Replace `YOUR_GOOGLE_MAPS_API_KEY_HERE` with your actual Google Maps API key:
```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="AIzaSyBOTY07CAuUf1998ZiKuLdOFhmyBzKlAOM" />
```

### 4. Configure IFTTT Webhook (Optional)

#### Set up IFTTT:
1. Go to [IFTTT](https://ifttt.com/) and create an account
2. Create a new applet
3. Choose "Webhooks" as the trigger service
4. Set event name as "bring_my_table"
5. Choose your desired action (email, notification, smart home device, etc.)
6. Get your webhook key from [IFTTT Webhooks settings](https://ifttt.com/maker_webhooks)

#### Add Webhook URL to the App:
1. Open `app/src/main/java/com/example/voicelocationapp/MainActivity.java`
2. Replace the IFTTT_WEBHOOK_URL constant:
```java
private static final String IFTTT_WEBHOOK_URL = "https://maker.ifttt.com/trigger/bring_my_table/with/key/YOUR_IFTTT_KEY_HERE";
```

### 5. Build and Run
1. Connect an Android device or start an emulator
2. Click "Run" in Android Studio or press Shift+F10
3. The app will install and launch on your device

## How to Use

1. **Grant Permissions**: When first launched, the app will request permissions for microphone and location access. Grant both permissions.

2. **Start Voice Recognition**: Tap the "Start Voice Command" button to begin listening.

3. **Say the Command**: Clearly say "bring my table" when the app is listening.

4. **View Results**: 
   - The recognized command will appear in the "Command" field
   - Your current location will be fetched and displayed in the "Location" field
   - The Google Map will update with a marker at your current location
   - If configured, your location will be sent to the IFTTT webhook

## App Structure

```
android-voice-location-app/
├── app/
│   ├── build.gradle                 # App-level dependencies and configuration
│   ├── proguard-rules.pro          # ProGuard configuration
│   └── src/main/
│       ├── AndroidManifest.xml     # App permissions and configuration
│       ├── java/com/example/voicelocationapp/
│       │   └── MainActivity.java   # Main app logic
│       └── res/
│           ├── layout/
│           │   └── activity_main.xml    # UI layout
│           ├── values/
│           │   ├── colors.xml          # Color definitions
│           │   ├── strings.xml         # String resources
│           │   └── themes.xml          # App themes
│           └── xml/
│               ├── backup_rules.xml    # Backup configuration
│               └── data_extraction_rules.xml
├── build.gradle                    # Project-level build configuration
├── gradle.properties             # Gradle properties
└── settings.gradle               # Project settings
```

## Key Components

### MainActivity.java
- **Voice Recognition**: Implements `RecognitionListener` for speech processing
- **Location Services**: Uses `FusedLocationProviderClient` for GPS location
- **Google Maps**: Implements `OnMapReadyCallback` for map integration
- **Network Requests**: Sends HTTP GET requests to IFTTT webhook
- **Permission Handling**: Manages runtime permissions for Android 6.0+

### UI Components
- **SupportMapFragment**: Displays Google Maps
- **TextViews**: Show status, recognized commands, and location details
- **Button**: Triggers voice recognition

## Permissions Required

- `RECORD_AUDIO`: For voice recognition
- `ACCESS_FINE_LOCATION`: For precise GPS location
- `ACCESS_COARSE_LOCATION`: For approximate location (fallback)
- `INTERNET`: For IFTTT webhook requests

## Troubleshooting

### Common Issues:

1. **Map not loading**: 
   - Verify Google Maps API key is correct
   - Ensure Maps SDK for Android is enabled in Google Cloud Console

2. **Voice recognition not working**:
   - Check microphone permissions
   - Ensure device has Google services installed
   - Test in a quiet environment

3. **Location not found**:
   - Check location permissions
   - Enable GPS/Location services on device
   - Test outdoors for better GPS signal

4. **IFTTT webhook not triggered**:
   - Verify webhook URL and key are correct
   - Check internet connection
   - Monitor Android logs for network errors

### Debugging:
- Use Android Studio's Logcat to view detailed logs
- Look for tags: "MainActivity", "IFTTT", and system logs
- Test individual components (voice, location, map) separately

## Customization

### Modify Voice Command:
In `MainActivity.java`, change the command detection:
```java
if (recognizedText.contains("your custom command")) {
    // Your logic here
}
```

### Change Map Style:
In the `onMapReady()` method, add map styling:
```java
mMap.setMapType(GoogleMap.MAP_TYPE_SATELLITE);
// or other map types
```

### Add More IFTTT Parameters:
Modify the webhook URL to include additional data:
```java
String urlString = IFTTT_WEBHOOK_URL + "?lat=" + latitude + "&lng=" + longitude + "&timestamp=" + System.currentTimeMillis();
```

## Security Notes

- Keep your Google Maps API key secure and restrict it to your app
- Don't commit API keys to version control
- Consider using Android's secrets management for production apps
- The IFTTT webhook URL contains your private key - keep it secure

## Requirements

- **Minimum SDK**: API Level 24 (Android 7.0)
- **Target SDK**: API Level 34 (Android 14)
- **Compile SDK**: API Level 34

## Dependencies

- Google Play Services Maps: 18.2.0
- Google Play Services Location: 21.0.1
- AndroidX AppCompat: 1.6.1
- Material Components: 1.10.0
- ConstraintLayout: 2.1.4

## License

This project is for educational purposes. Please ensure you comply with Google Maps API terms of service and IFTTT's terms when using this application.
