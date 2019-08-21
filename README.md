# Slack Tomorrow Theme

A slack theme using Tomorrow Night colors

# Note 

The latest version of Slack no longer uses this directory or these files.
I am working on a new fix.

# Installation

Add this code to the file `ssb-interop.js`:
- MacOS: `/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/ssb-interop.js`
- Windows: `C:\Users\<user>\AppData\Local\slack\app-<slack-version>\resources\app.asar.unpacked\src\static\ssb-interop.js`
- Linux: `/usr/lib/slack/resources/app.asar.unpacked/src/static/ssb-interop.js`

Place this code:
```js

/**
 * Add in the dark mode function
 */
document.addEventListener('DOMContentLoaded', function() {
    $.ajax({
        url: 'https://cdn.rawgit.com/mrjhnsn/slack-tomorrow-theme/master/custom.css',
        success: function(css) {
            $("<style></style>").appendTo('head').html(css)
        }
    });
});
```

Between these functions like so:

```js
 /**
   * Patch Node.js globals back in, refer to
   * https://electron.atom.io/docs/api/process/#event-loaded.
   */
  const processRef = window.process;
  process.once('loaded', () => {
    window.process = processRef;
  });

  window.perfTimer.PRELOAD_STARTED = preloadStartTime;

/**
 * Add in the dark mode function
 */
document.addEventListener('DOMContentLoaded', function() {
    $.ajax({
        url: 'https://cdn.rawgit.com/mrjhnsn/slack-tomorrow-theme/master/custom.css',
        success: function(css) {
            $("<style></style>").appendTo('head').html(css)
        }
    });
});

  // Consider "initial team booted" as whether the workspace is the first loaded after Slack launches
  ipcRenderer.once('SLACK_PRQ_TEAM_BOOT_ORDER', (_event, order) => {
    window.perfTimer.isInitialTeamBooted = order === 1;
  });
  ipcRenderer.send('SLACK_PRQ_TEAM_BOOTED'); // Main process will respond SLACK_PRQ_TEAM_BOOT_ORDER

  const resourcePath = path.join(__dirname, '..', '..');
  const mainModule = require.resolve('../ssb/main.ts');
  const isDevMode = loadSettings.devMode && isPrebuilt();

  init(resourcePath, mainModule, !isDevMode);
}
```

This fixes the issue for me on MacOs and intermittently on Linux.
There is something I am not smart enough to figure out yet that's causing these types of mods to not work on some builds.
