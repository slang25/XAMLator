# XAMLator live previewer for Xamarin.Forms

**XAMLator** is a live XAML previewer for Xamarin.Forms. Change something in your view's XAML in Visual Studio and you preview it live in your device or simulator!


# "But... Wait! Visual Studio has already a XAML preview, why do I need this stuff what that shitty name?"

Indeed! Visual Studio already has a XAML previewer, but it has some limitations.  The previewer only reads a XAML and tries to render it, whithout further context. If your view relies on any static initialization of the application, you are welcomed with a beatiful exception.

XAMLator works on a very different way, the XAML is sent to the device where the application is running and the application itself renders the XAML view, where the applicatios is now correctly initialized. Another benefit is that you preview it on a real device. An there is more... you can preview in several devices at the same time! An Android table, an iPhone X, an iPad, as many as you want!

## Xamarin Studio One-time Installation

Install the **Continuous Coding** add-in for Xamarin Studio in the **Add-in Manager** by adding a **New Gallery Repository** pointing at the URL:

	https://raw.githubusercontent.com/praeclarum/Continuous/master/Continuous.Client.MonoDevelop/AddinRepo

You will then be able to select the **Continuous Coding** add-in from the **IDE extensions** category.

<img src="https://raw.githubusercontent.com/praeclarum/Continuous/master/Documentation/AddAddinRepo.png" width="420px"/>

## Per-app Installation

1. Reference the [Continuous Coding nuget](https://www.nuget.org/packages/Continuous/).

2. Put this line of code somewhere in the initialization of your app (`AppDelegate.FinishedLaunching` or `Activity.OnCreate` are great places):

```csharp
#if DEBUG
new Continuous.Server.HttpServer(this).Run();
#endif
```

where `this` should refer to a `Context` on Android, and is ignored (can be anything or `null`) on iOS.

### Android Emulator Extra Step

If you want to run with the Android Emulator, you will have to forward the TCP port used by Continuous:

```bash
$ adb forward tcp:9634 tcp:9634
```

## Running

Run the debug version of your app. It should start as normal.

Verify that the Xamarin Inspector is installed and running by looking for the enabled cross-hair icon in the debug toolbar. If you see that while debugging, you're all set. If not, make sure you have a debug build and that the Inspector is installed.

### Running Snippets

Send snippets of code to be compiled, executed, and visualized using the **Visualize Selection** (Ctrl+Shift+Return) command.

### Continuously Coding a Class

You can mark a class to be monitored and automatically displayed whenever you edit it (and there are no code errors).

Move your cursor to be within a class and select the **Visualize Class** (Ctrl+Shift+C) command.


## Building

Build the solution `Continuous.sln`

### Prerequisties

Install the Xamarin Studio **Addin Maker** from the **Addin Development** group in the Gallery.


## How it Works

**Continuous** uses the class [`Mono.CSharp.Evaluator`](http://www.mono-project.com/docs/about-mono/languages/csharp/) to compile and execute code snippets while the app is running.

The IDE add-in allows the user to send snippets to this evaluator. The code is compiled and executed.

If the code produces a value (if it is an expression) then the value is automatically displayed.

The IDE communicates with the device using HTTP on port 9634. Requests and responses are simple JSON bundles.

