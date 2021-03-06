# Overview

This library is a helper for the migration from ClickOnce to [Squirrel.Windows](https://github.com/Squirrel/Squirrel.Windows)

The ClickOnce uninstall code is taken from [Wunder.ClickOnceUninstaller](https://github.com/6wunderkinder/Wunder.ClickOnceUninstaller)

# How to

The migration is super-simple, requiring only one method call in the ClickOnce version of the application and one method call in the Squirrel version of the application.

Create a new ClickOnce version of your application that you ship with the following code:

```cs
using (var updateManager = new UpdateManager("http://update.myapp.com"))
{
    var migrator = new InClickOnceAppMigrator(updateManager, "ClickOnceAppName");
    await migrator.Execute();
}
```

This installs the Squirrel version of your application and removes the ClickOnce shortcuts.

**Important note:** Make sure you include a copy of Squirrel.exe in your .nupkg file. If you don't do this, you won't have an Update.exe in you app folder, and you're gonna have a bad time.

In the new Squirrel version of your application, call the following code:

```cs
var migrator = new InSquirrelAppMigrator("ClickOnceAppName");
await migrator.Execute();
```

This uninstalls the rest of the ClickOnce application and leaves only the Squirrel version. Done!