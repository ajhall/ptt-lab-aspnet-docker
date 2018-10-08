# Make some changes

Instead of returning the hard-coded string `"value"` from `Get(int id)`, change it to return the number you passed in.

When you're done changing the code, go back to the command line. Your app should still be running here, with some logs shown based on your interactions with it. Press <kbd>CTRL</kbd>+<kbd>C</kbd> to stop your app, then press the up arrow to find `dotnet run` in your command-line history. Start your app up, then try a GET request to <https://localhost:5001/api/values/1> again. You should see `1` as a result now, and it should change to any number you enter as a parameter to `/values/{id}`.

You're probably thinking that each time you make a code change you'll have to go back to the command line, stop your app, and start it back up again. *There Has To Be A Better Way*, you say!

## .NET Core file watcher

Press <kbd>CTRL</kbd>+<kbd>C</kbd> to stop your app again. Then, run `dotnet watch --help` to learn about the `dotnet watch` command.

Run `dotnet watch run` to start your app again in watch mode. Go make another code change and keep an eye on the console window where your app is running. You'll see your app automatically exit and start up with your new code changes in place.