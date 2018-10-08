# 5. Create a release for deployment

When your app is ready to share, you'll want to get it to its users (in our case, usually the Ops team) in a ready-to-use form. The [`dotnet publish`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish) command compiles your app and drops the compiled output into a folder with all its dependencies. You can then take this folder and copy it to another computer to share your app.

If you run `dotnet publish` without any extra parameters, it'll make a few assumptions about what you want to do.

First of all, it'll publish to `bin\Release\netcoreapp2.1`. This isn't a big deal, but you might want to send your output somewhere easier to find.

Second, it'll use the `Debug` build configuration. This will produce binaries with extra debugging options enabled, which runs somewhat slower than if you publish in `Release` mode instead.

Third, it'll only publish the `.dll` files that make up your application, without any self-starting executable file. To run your app from this output, you'll need to run `dotnet WebAPI.dll` from your output folder. This means that your app will be portable across operating systems, but somewhat inconvenient to start up.

Run `dotnet publish --help` or check the [online documentation](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish) to learn how to specify your output directory, build configuration, and target runtime.

## Exercise 5

* Use `dotnet publish` to generate an executable version of your app
  * Set the build configuration to `Release`
  * Set the runtime to `win-x64`
  * Set the output directory to anything you want

## Tips

* You can also specify a `<RuntimeIdentifier>` in your `csproj` file to automatically target a certain runtime.
* You can build binaries for any platform supported by .NET Core (Windows, Mac, or Linux) from any OS by specifying a target runtime.

## Verify

* Open the output folder you specified and find `WebAPI.exe`.
* Stop any copies of your app that you might already have running.
* Run `WebAPI.exe` and use Postman to try making some requests to it.

