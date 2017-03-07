MogeanSDK Installation Instructions - iOS
If you are using CocoaPods (or Wish to use CocoaPods for integrating MogeanSDK), Please read Section A, otherwise jump to Section B
#Section A: Using CocoaPods to Integrate MogeanSDK
## Step-1: Initiate CocoaPods
```
$ pod init
```

## Step-2: Integrate MogeanSDK Pod into Pod file. 
MogeanSDK is available as a private pod. Open your Podfile and add the following Sources 

	source 'https://github.com/CocoaPods/Specs.git' 
	source 'https://github.com/mogean/MogeanSDKPodSpec'

Add Target MogeanSDK and your podfile will look something like this: 

	use_frameworks!

	target 'AppTargetName' 
	do 
	pod 'MogeanSDK' 
	end

## Step-3: Install MogeanSDK 

	$ pod install 

MogeanSDK is now integrated in your App. Make sure you close your XCode and reopen the project using .xcworkspace

#Section - B: Without using CocoaPod

Step-1: Clone this repository into your local system. 

Step-2: Drag and Drop MogeanSDK.framework into your workspace.

The recommended approach is to setup the framework in your Build Settings under Linked Frameworks and Libraries. If you include the framework under Embedded Binaries you may need to remove unneeded architectures when submitting your archive. If you see this error when submitting your archive see section "Removing Unneeded Architectures Before Submitting Archive to iTunesConnect"


#Section - C: Using MogeanSDK

##Instructions using Swift
In your App Delegate, import MogeanSDK:

	import MogeanSDK

Create a Shared Instance by calling: 

	let mogeanSharedInstance:Mogean = Mogean.sharedInstance 

In your didFinishLaunchingWithOptions function, initialize Mogean by setting the MogeanID, and consumerKey

	mogeanSharedInstance.peelID = "<Your MogeanID Issued by Mogean>"
	mogeanSharedInstance.mogeanConsumerKey="<Your Mogean Consumer Key Here>"


You can send Custom Event Types during App Launch, App Background and App Exit. To do this, please call:

	mogeanSharedInstance.setCustomEvent(MogeanEventTypes.AppLaunch.rawValue)
	mogeanSharedInstance.setCustomEvent(MogeanEventTypes.AppBackground.rawValue)

or

	mogeanSharedInstance.setCustomEvent(MogeanEventTypes.AppExit.rawValue)

##Instructions using Objective-C

In your AppDelegate.h
	
	@import MogeanSDK;
	
and then 

	@property (strong, nonatomic) Mogean *mogean;


In your AppDelegate.m
	
	Mogean *mogean = [Mogean sharedInstance];
    
	mogean.peelID = @"<Your MogeanID Issued by Mogean>";
	mogean.mogeanConsumerKey=@"<Your Mogean Consumer Key Issued by Mogean>";

In your Target "Build Settings", find "Build Options" and set "Embedded Content Contains Swift Code" to YES. 

# Section - D: Location Authorization Instructions

In your info.plist file, be sure to add key "NSLocationAlwaysUsageDescription" and give a proper message string description for your users when requesting for Location Always Usage authorization.

You are ready to use Your MogeanSDK Now. 

# Section - E: Removing Unneeded Architectures Before Submitting Archive to iTunesConnect

The MogeanSDK is an embedded dynamic framework so the single framework contains the architectures requried to run the functionality on devices as well as the simulator. The simulator architectures need to be removed when submitting to iTunesConnect.

Within your Target then in Build Phases below Embed Frameworks, where you see MogeanSDK.framework listed, add in a Run Script section.

Make sure the Shell is set to: ```/bin/sh```

Add the following script:

```
	APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"

	# This script loops through the frameworks embedded in the application and
	# removes unused architectures.
	find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
	do
	FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
	FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
	echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"

	EXTRACTED_ARCHS=()

	for ARCH in $ARCHS
	do
	echo "Extracting $ARCH from $FRAMEWORK_EXECUTABLE_NAME"
	lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"
	EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")
	done
	
	echo "Merging extracted architectures: ${ARCHS}"
	lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"
	rm "${EXTRACTED_ARCHS[@]}"
	
	echo "Replacing original executable with thinned version"
	rm "$FRAMEWORK_EXECUTABLE_PATH"
	mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"
	
	done
```

# Section - F: Submitting to App Store

The MogeanSDK utilizes iOS Advertising Identifier (IDFA). During the App Store submission process you will need to answer "Yes" to this question. The MogeanSDK does comply with the usage limitations of the Advertising Identifier and the Limit Ad Tracking setting.

After answering "Yes" that the app uses the IDFA, then select the following options below:

	DO NOT SELECT - Serve advertisements within the app

Note: the above setting should not be selected based on the MogeanSDK functionality, but if your app currently already does use the IDFA for purposes of serving advertisements within the app, then you should select this option.
	
	SELECT - Attribute this app installation to a previously served advertisement
	SELECT - Attribute an action taken within this app to a previously served advertisement

You will then SELECT the final checkbox about the Limit Ad Tracking setting in iOS. The MogeanSDK does monitor this setting by the end user and is taken into account during data processing and delivery to ensure compliance.
