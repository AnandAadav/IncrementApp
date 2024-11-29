Increment App

Bridging Flutter and Native iOS (Swift)


Summary

I created a simple Swift app that includes an incrementCount function to increment a counter.
I set up a Flutter project and added a platform channel to communicate with the Swift code.
The Flutter app calls the incrementCount method in Swift to increment the count and display the result.
You now have an app where Flutter can communicate with Swift via platform channels, allowing you to call native functionality from Flutter.

step-by-step guide

1. Create an Increment App in Swift (iOS)
First, let's create the Swift code for the iOS part of the app.

Steps:
Create a new iOS app in Xcode:

Open Xcode and create a new iOS project using the "App" template.
Select "Swift" as the language and "UIKit" as the user interface option.
Implement Increment Logic in Swift:


2. Set Up the Flutter Project
Now, let's create the Flutter app that will call the Swift code to get and increment the count.

Steps:

Create a New Flutter Project: In your terminal, create a new Flutter app:

flutter create increment_app
cd increment_app
Open the Flutter Project: Open the project in your preferred editor (VSCode, Android Studio, etc.).

Navigate to the iOS Part: Change to the iOS directory:

cd ios

3. Add Swift Code to the Flutter Project
Now we’ll modify the Flutter project to include the native Swift increment functionality and bridge it via a platform channel.

Steps:
Enable Swift Support in Your Flutter Project:

flutter create .
This will set up Swift support in your Flutter project.

Add Swift Code for Increment Functionality:

Open the Runner.xcworkspace file in Xcode, and add a new Swift file (e.g., Increment.swift) under the Runner target.

In Increment.swift, add the following code:


This class has an incrementCount method that increments the count and returns the updated value.

Update AppDelegate.swift:

Now, modify the AppDelegate.swift file to use the platform channel to communicate with the Flutter app. This will allow Flutter to invoke the native Swift incrementCount method.

Open AppDelegate.swift and modify it as follows:

import UIKit
import Flutter

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
    private var incrementer: Incrementer!
    private var incrementChannel: FlutterMethodChannel?

    override func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        let controller = window?.rootViewController as! FlutterViewController
        incrementChannel = FlutterMethodChannel(name: "com.example.increment", binaryMessenger: controller.binaryMessenger)
        
        incrementChannel?.setMethodCallHandler { (call, result) in
            if call.method == "incrementCount" {
                if self.incrementer == nil {
                    self.incrementer = Incrementer()
                }
                let newCount = self.incrementer.incrementCount()
                result(newCount)
            } else {
                result(FlutterMethodNotImplemented)
            }
        }

        return super.application(application, didFinishLaunchingWithOptions: launchOptions)
    }
}
This code creates a platform channel named com.example.increment, listens for calls to the incrementCount method, and responds with the updated count.

4. Flutter-to-Native Communication
Now, let’s modify the Flutter app to call the Swift code and display the updated count.

Steps:
Update Flutter Code to Call the Increment Method:

Open the lib/main.dart file and update it to call the incrementCount method on the native Swift code:



Uses a MethodChannel to invoke the incrementCount method in Swift.
Displays the current count and updates it every time the increment button is pressed.
5. Run the App
Run the App: Go back to the root directory of your Flutter project and run the app:

flutter run

Test the Increment Functionality: When you press the "Increment" button, the Flutter app will communicate with the native Swift code, increment the count, and display the updated count in the Flutter interface.



