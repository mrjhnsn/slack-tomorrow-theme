# Slack Tomorrow Theme

A slack theme using Tomorrow Night colors

# Installation

Add this code to the file `ssb-interop.js`:
- MacOS: `/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/ssb-interop.js`
- Windows: `C:\Users\<user>\AppData\Local\slack\app-<slack-version>\resources\app.asar.unpacked\src\static\ssb-interop.js`
- Linux: '/usr/lib/slack/resources/app.asar.unpacked/src/static/ssb-interop.js'

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


/**
 * Add in the dark mode function
 */

document.addEventListener('DOMContentLoaded', function() {
    $.ajax({
        url: 'https://raw.githubusercontent.com/mrjhnsn/slack-tomorrow-theme/master/custom.css',
        success: function(css) {
            $("<style></style>").appendTo('head').html(css)
        }
    });
});



/**
 * loadSettings are just the command-line arguments we're concerned with, in
 * this case developer vs production mode.
 */
const loadSettings = window.loadSettings = assignIn({},
  require('electron').remote.getGlobal('loadSettings'),
  { windowType: 'webapp' }
);
```

This fixes the issue for me on MacOs and intermittently on Linux.
There is something I am not smart enough to figure out yet that's causing these types of mods to not work on some builds.
