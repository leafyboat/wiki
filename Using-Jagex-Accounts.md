### Using Jagex Accounts

If you have converted your account to a Jagex Account and can no longer login directly to RuneLite, but want to do RuneLite development, follow this guide:

1. Make sure your _RuneLite launcher_ is version 2.6.3 or newer
2. For Windows, run `RuneLite (configure)` from the start menu. Otherwise, pass `--configure` to the launcher (i.e. `/Applications/RuneLite.app/Contents/MacOS/RuneLite --configure` on Mac).
3. In the `Client arguments` input box add `--insecure-write-credentials`
4. Click Save
5. Launch RuneLite via the Jagex launcher. RuneLite will write your launcher credentials to `.runelite/credentials.properties`. These credentials can be used to login into your account directly, bypassing your password. **Do not share this file with anyone.**
6. Launch RuneLite client (eg. via the IDE) and it will use the saved credentials.

Once you've finished development you can delete the `credentials.properties` file to return your Runelite back to normal. If for any reason you need to invalidate the credentials, you can use the "End sessions" button under account settings on runescape.com.

### macOS 14 (Sonoma) and later

If RuneLite was installed by the Jagex Launcher, its app bundle is at `~/Library/Application Support/Jagex Launcher/Games/Old School RuneScape/RuneLite/RuneLite.app` rather than `/Applications`, so use that path when passing `--configure` in step 2.

Saving from the Configure window can also fail silently. On macOS the launcher writes `settings.json` relative to its working directory, which for a Jagex Launcher install is inside the app bundle itself (`RuneLite.app/Contents/Resources`), and macOS 14 added App Management protection, which blocks writes inside signed application bundles. The Configure window still shows the arguments you typed and closes normally, so the only symptom is that `credentials.properties` never appears in step 5.

To confirm this is what happened, check `~/.runelite/logs/launcher.log` for:

```
ERROR n.runelite.launcher.LauncherSettings - unable to save launcher settings!
java.nio.file.FileSystemException: ... -> settings.json: Operation not permitted
```

Subsequent launches will also log `client arguments: none`.

To work around it, grant the app you run the launcher from (Terminal, or whichever terminal you use) **App Management** permission under System Settings → Privacy & Security → App Management, then run `--configure` again and save. Note that this writes inside the app bundle and invalidates its code signature; the Jagex Launcher may replace the bundle when it updates RuneLite, which undoes the change.
