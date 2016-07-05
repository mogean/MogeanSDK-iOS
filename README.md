MogeanSDK Installation Instructions - iOS
If you are using CocoaPods (or Wish to use CocoaPods for integrating MogeanSDK), Please read Section A, otherwise jump to Section B
#Section A: Using CocoaPods to Integrate MogeanSDK
## Step-1: Initiate CocoaPods 
$ pod init
## Step-2: Integrate MogeanSDK Pod into Pod file. 
MogeanSDK is available as a private pod. Open your Podfile and add the following Sources 

	source 'https://github.com/CocoaPods/Specs.git' 
	source 'https://github.com/mogean/MogeanSDKPodSpec'

Add Target MogeanSDK and your podfile will look something like this: 

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


#Section - C: Using MogeanSDK

Instructions using Swift
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

Instructions using Objective-C

In your AppDelegate.h
	
	@import MogeanSDK;
	
and then 

	@property (strong, nonatomic) Mogean *mogean;


In your AppDelegate.m
	
	Mogean *mogean = [Mogean sharedInstance];
    
	mogean.peelID = @"<Your MogeanID Issued by Mogean>";
	mogean.mogeanConsumerKey=@"<Your Mogean Consumer Key Issued by Mogean>";

# Section - D: Location Authorization Instructions

In your info.plist file, be sure to add key "NSLocationAlwaysUsageDescription" and give a proper message string description for your users when requesting for Location Always Usage authorization.

You are ready to use Your MogeanSDK Now. 

# Section - E: Submitting to App Store

The MogeanSDK utilizes iOS Advertising Identifier (IDFA). During the App Store submission process you will need to answer "Yes" to this question. The MogeanSDK does comply with the usage limitations of the Advertising Identifier and the Limit Ad Tracking setting.

After answering "Yes" that the app uses the IDFA, then select the following options below:

	DO NOT SELECT - Serve advertisements within the app

Note: the above setting should not be selected based on the MogeanSDK functionality, but if your app currently already does use the IDFA for purposes of serving advertisements within the app, then you should select this option.
	
	SELECT - Attribute this app installation to a previously served advertisement
	SELECT - Attribute an action taken within this app to a previously served advertisement

You will then SELECT the final checkbox about the Limit Ad Tracking setting in iOS. The MogeanSDK does monitor this setting by the end user and is taken into account during data processing and delivery to ensure compliance.
