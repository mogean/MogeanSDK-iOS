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
In your App Delegate, import MogeanSDK 
import Mogean

Create a Shared Instance by calling: 
let mogeanSharedInstance:Mogean = Mogean.sharedInstance 

In your didFinishLaunchingWithOptions function, initialize Mogean by setting the MogeanID, comsumerKey and Secret
mogeanSharedInstance.peelID = "<Your MogeanID Issued by Mogean>"
mogeanSharedInstance.mogeanConsumerKey="<Your Mogean Consumer Key Here>"
mogeanSharedInstance.mogeanConsumerSecret="<Your Mogean Consumer Secret Here>"

You can send Custom Event Types during App Launch, App Background and App Exit. To do this, please call
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
    mogean.mogeanConsumerSecret=@"<Your Mogean Consumer Secret Issued by Mogean>";


In your info.plist file, be sure to give a proper message for "NSLocationAlwaysUsageDescription" key

You are ready to use Your MogeanSDK Now. 
