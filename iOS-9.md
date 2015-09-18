##Discussion##
For publishers compiling their apps against the iOS 9 SDK (Xcode 7), AdColony's 2.6 SDK is a requirement.
v2.6.0 of the AdColony SDK requires no API-usage modifications and, as such, will essentially be a drag-and-drop update. The only new requirement is that publishers make a couple of modifications to their apps' plists in order to be compatible with the changes introduced in iOS 9. Please note that failure to follow our instructions for disabling App Transport Security (ATS) will result in ad-serving being disabled for your application.

##Integration Instructions##
Publishers will have to add two entries to their plists in order for the AdColony SDK to function at its full potential. The first entry, which is necessary to disable ATS for AdColony, is a strict requirement. Please note that if this entry is not present, or it is incorrect, the SDK will not be able to talk to our network - the side effect of this being complete lack of ad fill and, in turn, severe revenue implications.

###Disabling ATS###
App Transport Security (ATS) requires apps to make secure network connections via SSL and enforces HTTPS connections through its requirements on the SSL version, encryption cipher, and key length. We are currently working with all of our partners in an effort to become ATS-compatible. At this time, however, we require that publishers disable ATS for AdColony. This can be done two ways; please see below.

**Option 1**  
The easiest way to turn off ATS for AdColony is to disable it for the entire application by adding the following entry to the app's plist:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

**Option 2**  
Disabling ATS for AdColony does not require that it be turned off for the entire application, however. In an effort to be aligned with Apple's recommendations around ATS, publishers may want to ensure their apps are as ATS-compliant as they can be. If you are one of those publishers, then Option 2 is what you want. To disable ATS for AdColony but keep it enabled for other domains, simply add the XML from Option 1 to your app's plist (which disables ATS for the entire application), and then add an exception for each domain known to be ATS-compliant. Please refer to the example below.

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
    <key>NSExceptionDomains</key>
    <dict>
        <key>example.com</key>
        <dict>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

###Using canOpenURL:###
Apple has also restricted usage of the `canOpenURL:` API, which AdColony leverages to land users in certain apps from our Dynamic End Cards (DECs). For example, one of our ad units could be for a new movie, and the associated DEC may present functionality to the user that allows them to send a tweet about it using the Twitter app. This kind of functionality is still possible in iOS 9, but publishers must enable it for the applications AdColony links to by authorizing each app’s URL scheme in their plist. Note that if the schemes are not added, users will be taken to the app’s website instead, which may result in an undesirable user experience. In order to enable deep-linking for the apps the AdColony SDK uses, please add the following entry to your apps plist:

```xml
<key>LSApplicationQueriesSchemes</key>
    <array>
        <string>fb</string>
        <string>instagram</string>
        <string>tumblr</string>
        <string>twitter</string>
    </array>
</key>
```

##Notes##
* Important: Disabling ATS for AdColony is a strict requirement. Failure to do so will result in ads being turned off for your entire application.
* v2.6.0 of the AdColony SDK is compiled with bitcode.