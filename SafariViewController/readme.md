SafariViewController (iOS 9)
====================

It's relatively simple to get the new iOS 9 `SFSafariViewController` working with Xamarin. Sample in both C# and F#.

C#
--

```
using SafariServices;
using Foundation;
```

```
openButton.TouchUpInside += (sender, e) => {
	var sfvc = new SFSafariViewController (new NSUrl("http://xamarin.com"),true);
	PresentViewController(sfvc, true, null);
};
```

and

```
[Foundation.Export ("safariViewControllerDidFinish:")]
public void DidFinish (SFSafariViewController controller)
{
	DismissViewController (true, null);
}
```

![](Screenshots/safariviewcontroller.png)

F#
--

**WARNING:** no idea if this is "good" F# or not.

```
namespace SafariViewDemoFS

open System
open CoreGraphics
open SafariServices
open Foundation
open UIKit

[<Register ("ViewController")>]
type ViewController () as x =
    inherit UIViewController () 

    let showSafari () =
        let sfvc = new SFSafariViewController (new NSUrl("http://xamarin.com"), true)
        x.PresentViewController (sfvc, true, null)

    override x.DidReceiveMemoryWarning () =
        base.DidReceiveMemoryWarning ()

    override x.ViewDidLoad () =
        base.ViewDidLoad ()
        let b = UIButton.FromType(UIButtonType.System)
        b.Frame <- new CGRect (nfloat 10.0f, nfloat 40.0f, nfloat 150.0f, nfloat 40.0f) 
        b.SetTitle("Safari View Controller", UIControlState.Normal)
        b.TouchUpInside.Add (fun _ -> showSafari())
        x.View.Add b

    override x.ShouldAutorotateToInterfaceOrientation (toInterfaceOrientation) =
        if UIDevice.CurrentDevice.UserInterfaceIdiom = UIUserInterfaceIdiom.Phone then
           toInterfaceOrientation <> UIInterfaceOrientation.PortraitUpsideDown
        else
           true
```
