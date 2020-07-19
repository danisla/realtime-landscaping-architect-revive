## How to run Realtime Landscape Architect with Revive

To run Realtime Landscape Architect with Revive, you have to inject the DLLs into both the main exe and the realtime viewer executable.

  - The main exe needs injection because it checks for the Rift when launching the Walkthrough view.
  - The actual VR experience is handled by the realtime viewer subprocess. This file is "rv trial.exe" in the trial download. This exe also needs DLL injection.
  - To get the injection to work on both exes, a shim batch file is used to intercept the call to realtime viewer and prepend it with the ReviveInjector.exe

The hardest part of this hack was the command argument passing and spaces in filenames, go figure.

Once the shortcuts and shim script described below is installed, the Walkthrough experience runs just fine in SteamVR on my Oculus Quest through Virtual Desktop. 

I have not figured out inputs yet with my Oculus Quest, so your milage may very.

## Tutorial

1. Download and install the [Realtime Landscape Architect Trial](https://www.ideaspectrum.com/free-landscape-design-software-trial/)

2. Download and install [Revive](https://github.com/librevr/revive/releases)

3. Create a shortcut on the Desktop to the revive injector.

    a. Set the __Target__ to: `"C:\Program Files\Revive\ReviveInjector.exe" "C:\Program Files\Realtime Landscaping Architect 2020 Trial\Realtime Landscaping Architect Trial.exe"`
    
    b. Set the __Start in__ to: `"C:\Program Files\Revive"`

<img src="https://raw.githubusercontent.com/danisla/realtime-landscaping-architect-revive/master/images/rla_revive_shortcut.png" width="320">

4. Rename `C:\Program Files\Realtime Landscaping Architect 2020 Trial\rv trial.exe` to `C:\Program Files\Realtime Landscaping Architect 2020 Trial\rv trial orig.exe` 

5. Create a batch file named `rvtrial-revive.bat`:

```bat
@echo off
rem Short name path to original RV file "rv trial orig.exe"
set rv_orig="C:\PROGRA~1\REALTI~1\RVTRIA~1.EXE"

"C:\Program Files\Revive\ReviveInjector.exe" "%rv_orig%" "\"%~1\"" "\"%~2\"" "\"%~3\"" "\"%~4\"" "\"%~5\""

rem Uncomment below to see ReviveInjector debug output
rem set /p id="Enter to exit"
```

> NOTE: the short path to the `rv trial orig.exe` and the quoted positional arguments are key to making the wrapper script work with ReviveInjector.exe.

6. Download and install [Advanced BAT to EXE Converter](https://www.battoexeconverter.com/#downloadbattoexe)

7. Compile the bat file to exe and copy the output exe to `C:\Program Files\Realtime Landscaping Architect 2020 Trial\rv trial.exe`

8. Open the Oculus app, Steam VR and Revive Dashboard. 

> NOTE: revive usually launches these automatically but I prefer to have them open and ready before running my apps.

9. Run the shortcut to launch the app with the injector created earlier and open your design.

10. Click __Walkthrough__ at the bottom and check the `Use Oculus Rift` option then click __OK__ to run.

![](https://raw.githubusercontent.com/danisla/realtime-landscaping-architect-revive/master/images/rla_walkthrough_launch.png)

> The walkthrough should launch in Steam VR.

![](https://raw.githubusercontent.com/danisla/realtime-landscaping-architect-revive/master/images/rla_walkthrough_steam_vr.png)
