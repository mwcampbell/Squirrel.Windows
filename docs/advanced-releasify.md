## Advanced package creation

Here is a simplified help output specifically around creating releases:

```
Usage: Squirrel.exe command [OPTS]
Creates Squirrel packages

Commands
      --releasify=VALUE      Update or generate a releases directory with a
                               given NuGet package

Options:
  -h, -?, --help             Display Help and exit
  -r, --releaseDir=VALUE     Path to a release directory to use with Releasify
  -p, --packagesDir=VALUE    Path to the NuGet Packages directory for C# apps
      --bootstrapperExe=VALUE
                             Path to the Setup.exe to use as a template
  -g, --loadingGif=VALUE     Path to an animated GIF to be displayed during
                               installation
  -n, --signWithParams=VALUE Sign the installer via SignTool.exe with the
                               parameters given
```

### Loading GIFs

Squirrel installers don't have any UI - the goal of a Squirrel installer is to install so blindingly fast that double-clicking on Setup.exe *feels* like double-clicking on an app shortcut. Make your installer **fast**.

However, for large applications, this isn't possible. For these apps, Squirrel will optionally display a graphic as a "splash screen" while installation is processing, but only if installation takes more than a pre-set amount of time. This will be centered, backed by a transparent window, and can optionally be an animated GIF. Specify this via the `-g` parameter.

### Setup.exe Signing

Signing your installer with a valid code signing certificate is one of the most important things that you need to do for production apps. Both IE SmartScreen as well as virus scanning software will give a significant amount of "points" to apps that are signed correctly, preventing your users from getting scary dialogs.

Acquire a code-signing certificate - it's recommended to get a Windows Error Reporting-compatible certificate, see [this MSDN article](http://msdn.microsoft.com/en-us/library/windows/hardware/hh801887.aspx) for more information, then pass the `-n` parameter, which are the parameters you would pass to `signtool.exe sign` to sign the app.

Squirrel makes signing easy, as it signs all of your application's executables as *well* as the final generated Setup.exe.

An example invocation including both of these features would be something like:

```
Squirrel --releasify MyApp.nupkg -g .\loading.gif -n "/a /f CodeCert.pfx /p MySecretCertPassword"
```