##BoldRocket Objective-C Style Guide

This style guide serves to outline the coding conventions and style for all Bold Rocket Objective-C projects. The style guide should always be adhered to, even in the case of small proof of concepts or prototype applications.

## Aim

The aim of this style guide is to standardise the way in which Objective-C is written, which subsequently reduces friction in reading it, making it more maintainable and therefore of a better quality.

The guide does not aim to define an applications architecture or any implementation details, these are domain specific problems which should be driven by the software architects and developers.

## Resources
* [Cocoa Coding Guidelines](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [objc.io](http://www.objc.io/)
* [NSHipster](http://nshipster.com/)
* [iOS Dev Weekly](http://iosdevweekly.com/)
* [Github Trending Objective-C Repositories](https://github.com/trending?l=objective-c)
* [NSBlog](https://www.mikeash.com/pyblog/)

## Table of Contents
* [Xcode Project](#xcode-project)
* [Header Organisation](#header-organisation)
* [Implementation Organisation](#implementation-organisation)
* [Types](#types)
* [Variables](#variables)
* [Classes and Methods](#classes-and-methods)
* [Properties](#properties)
* [Delegates](#delegates)
* [Localisation](#localisation)
* [Constants](#constants)
* [Initialization](#initialization)
* [Conditionals](#conditionals)
* [Future Revisions](#future-revisions)

## Xcode Project
* The underlining file and folder system should match the project structure inside of Xcode. If a group is created in Xcode an underlining folder should exist in the same place on the file system.

* Xcassets, where available, should be used to manage image assets.

* Image files should be descriptive of their screen function but should avoid visually describing their look. Filenames should be all lower case and spaces in file names should be replaced with hyphens.

**Example**
```obj
login-button@2x.png
clear-menu-list-icon@2x.png
```

* The Application Delegate should be called “AppDelegate” in all projects, there’s no requirement to rename this to be application specific.

* XIB files should not have “Controller” as part of their file name but can optionally include “View” or “Window”.

**Example**
```objc
LoginView.xib
Register.xib
```
* All class file names should start with a capital letter, along with all Xibs and Storyboard files.

**Example**
```objc
LoginViewController.m
WebServiceInterface.h
```

* Projects should use the bundle ID in the format of com.boldrocket.[Client][Programme][Project] this helps keep naming conventions consistent across the provisioning portal.

**Example**
```objc
com.boldrocket.XYZGlobalProgrammeLocalProject
```

* For frameworks and reusable classes class level prefixes should be used to avoid collisions – as per [Apple guidelines](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/Conventions/Conventions.html) 3 letters or more should be used for 3rd party libraries.

**Example**
```objc
BRTLoginViewController.m
RDNUrlCache.m
```

## Header Organisation
* Avoid declaring methods in the header which are not publicly required

* Avoid `#import`ing in header files unless explicitly required. Instead opt for forward class declaration via `@class`. This leads to [cleaner headers and faster compile times](http://qualitycoding.org/file-dependencies/). The `@class` reference should be declared below the `#import` statements but above the `@interface` statement with a blank line separating each category.

**Example**
```objc
 #import <UIKit/UIKit.h>
 #import <Foundation/Foundation.h>

 @class MBProgressHUD; 

 @interface LoginViewController : UIViewController
```

* Always import framework headers first, followed by a blank line and then the class specific imports.

**Example**
```objc
 #import <UIKit/UIKit.h>
 #import <Foundation/Foundation.h>
 #import <CoreLocation/CoreLocation.h>
 
 #import "LoginViewController.h"
 #import "IntroductionViewController.h"
```

## Implementation Organisation
* Conditional and loop bracing should start on the same line and end on a new line.

**Example**
```objc
if (user.isLoggedIn) {
...
}
```

* Method level braces should always be on included on their own line.

**Example**
```objc
 - (IBAction)submitLoginCredentials:(UIButton *)sender
{
...
}
```

* Always use braces for if / for / while / else etc. statements. Readability and consistency are a fair trade-off in all cases, even in the case of early returns.

**Example**
```objc
if (!user.isValid) {
...
}
```

* Use Pragma mark to separate method sets, the below set provides a breakdown of the default set which should be used, additional ones can be added as required.
* Lifecyle – `viewDidLoad`, `init`, `dealloc` etc.
 * Public – All public methods required for the class to function.
 * Actions – `IBActions`
 * Protocols – named individually by their class name
 * Private / Utility – For any private methods and class specific utility methods

**Example**
```objc
#pragma mark – Lifecycle

- (void)viewDidLoad
{
...
}

#pragma mark – Public

- (void)submitLoginCredentials
{
...
}

#pragma mark – Actions

- (IBAction)submitLoginCredentials:(id)sender
{
...
}

#pragma mark – UITableViewDelegate

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
...
}

#pragma mark – UITableViewDataSource

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
...
}

#pragma mark – Private / Utility

- (void)clearFormTextFields
{
...
}
```

* Use method returns early to help keep conditional logic clear.

**Example**
```objc
- (void)processRequest
{
    if (!user.isValid) {
        return;
    }

    // continue with flow
}
```

* There should only be a one line space between methods for consistency as well as a one line space between the final method brace and the `@end` directive.

**Example**
```objc
- (void)hideAdditionalTextFields
{
...
}

- (void)clearFormTextFields
{
...
}

@end
```

* Method declarations should always have a space after the class vs instance indicator (-/+).

**Example**
```objc
+ (void)registerForPushNotifications;
- (void)clearDefaultRequestParameters;
```

## Types
* All Objective-C primitive types should be used over their C counterpart where available - this aids in future proofing code.

* `NSInteger` and `NSUInteger` should be used over int, the only exception is for loop indices.
```objc
for (int i = 0; i < 10; i++) {
...
}
```

* `CGFloat` should be used over float.

* The auxiliary and additional types such as NSTimeInterval should be used (where relevant) over doubles.

## Variables
* Variable names should always be camel cased with the first letter lowercase and the pointer prefixing the variable name.

**Example**
```objc
NSString *firstName;
NSString *lastName;
```

* Variables should be named verbosely in line with Apple’s conventions. *The only exception is for loop indices.
UIColor *configuredColorForView;

* When declaring `IBOutlet` the variable name should also be suffixed with the control type - this avoids issues with ambiguity when referencing the object later.

**Example**
```objc
self.usernameTextField.text = @"Chuck Norris";
[self.loginFormScrollView setContentOffset:aPoint animated:YES];
```

* Use appropriate prefixes when working with typedef structures

**Example**
```objc
typedef enum {     
    BRTLoginStyleLight,     
    BRTLoginStyleDark 
} BRTLoginStyle;
```

## Classes and Methods
* All View Controllers class names should be suffixed with ViewController.

**Example**
```objc
LoginViewController.m
RegisterViewController.m
```

* Class factory methods should be created where relevant, in-line with Apple’s recommendation and best practices. This aids and simplifies the process of object creation by the client.

**Example**
```objc
+ (id)dateWithTimeIntervalSinceNow:(NSTimeInterval)secs;
+ (id)userWithFirstName:(NSString *)firstName lastName:(NSString *)lastName; 
```

* When invoking multiple methods on a single object limit to a maximum of 2 for readability and make sure there’s a space between each call.

**Example**
```objc
[DataManager sharedInstance] fetchFriends];
```

## Properties
* Avoid declaring instance variables and opt for properties instead.

* Dot-notation should be used to access and mutate properties.

**Example**
```objc
user.firstName = @”John”;
[DataManager sharedInstance].clientId;
```

* Nonatomic first, for consistency.

**Example**
```objc
@property (nonatomic, strong) MPHostServiceBrowser *serviceBrowser;
@property (nonatomic, strong) NSMutableArray *connectionList;
```

* If values need to be set on a readonly property declared in a header file declare the property with the readwrite attribute in the .m file.

**Example**
```objc
@property (nonatomic, strong, readwrite) NSString *userId;
```

## Collections
* Collection literals should always be used for non mutable items such as `NSArray`, `NSDictionary` etc.

* Access collection literals using the short syntax and the mutable counterpart using the family of `objectAtIndex` methods. This makes it clear what type of collection is being accessed.

**Example**
```objc
NSString *user = self.userList[0]; 
NSString *user = [self.userList objectAtIndex:0];
```

* When working with large dictionaries (e.g. JSON responses) opt to create class representations (models) of the data instead, this will help with code completion and compiler checks and avoid any keyName mistakes.

## Delegates
* When defining `delegate` callbacks the format should be in line with the standard UIKit conventions which starts with the object responsible for the delegation, the auxiliary verb (will/should/did/has/should) and the event.

**Example**
```objc
scrollViewDidScroll:
webView:shouldStartLoadWithRequest:navigationType:
```

* `Delegate` / `Data Sources` should be declared explicitly in code, rather than through Interface Builder. Delegates should be declared together in an associated “setup” method. This is useful as you’re likely to spend more time in code than interface builder so debugging can be performed more conveniently.

## Localisation
* For apps across regions use NSLocalized string for all user displayed string output. You may optionally choose to use them in non-regional if also required because of the [flexibility it provides](http://nshipster.com/nslocalizedstring/).

**Example**
```objc
textField.placeholder = NSLocalizedString(@"Username", "Username Placeholder");
```

## Constants
* Opt to declare constants in `.h` & `.m` files as `static` constants and not `#define` – this should also include a header reference where relevant.

**Example**
```objc
// .h 
extern NSString * const BRTRootUrl;

// .m
static NSString * const BRTRootUrl = @"http://www.boldrocket.com/";  
```

## Initialization
* For common initializers always use commonInit, this should be consistent across all relevant classes within the project.

**Example**
```objc
- (void)commonInit 
{ 
    // Common init code here!
}  

- (id)initWithFrame:(CGRect)aRect 
{     
    if ((self = [super initWithFrame:aRect])) {        
        [self commonInit];     
    } 
    return self; 
}  

- (id)initWithCoder:(NSCoder*)coder 
{     
    if ((self = [super initWithCoder:coder])) {         
        [self commonInit];     
    }     
    return self; 
}
```

## Conditionals
* Ternary operators should be kept to a maximum of 2 conditions.

**Example**
```objc
BOOL isUserActive = YES;
result = isUserActive ? valueIfTrue : valueIfFalse;
```

## Future Revisions

While endeavouring to produce an initial version of the style guide it’s fully expected this will be require further revisions going forward, below highlighted is discussion points for potential inclusion at a future date.

* instancetype vs id
* CGRect functions vs direct CGRect access
* Xclode Project Structuring
