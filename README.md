# Summary
 An Adobe ExtendScript compatible log file constructor with some built in
 convenience functions in a UMD wrapper for cross-compatability with AMD and
 node.js require.

Tries to be flexible but automate most things.

# Features
- Works with node.js require, AMD(probably), and vanilla ExtendScript.
- Tries to make a log file in ./logs with fallback to ~/Desktop/ExtendScript_Log_UnsavedScripts/
- Tries to clean up old files automatically (or keep them foreverrr)
- Plays nice with [ExtendScript_Log](https://github.com/MaxJohnson/extendscript_log)

# Import
## NPM
If running Node NPM, you can `npm install ExtendScript_Log` to add to your node_modules folder
## github
Clone or download the repo and copy the extendscript_logfile.jsxinc to your project

# Include

## NPM
`var LogFile = require("ExtendScript_LogFile");`

## AMD
I don't know but it's probably not difficult? Firmly in the untested-but-should-work category

## ExtendScript
### Eval into environment
`$.evalFile("<path>/extendscript_logfile.jsxinc")`

### Include in scripts
`//@include "<path>/extendscript_logfile.jsxinc"`

### concatinate or copy-paste directly
Add to a build script or, I dunno, just copy-pasta it in there?

# Use:

## Make new LogFile object
make a new log file and you get a separate instance
```
var myLogFile = new ExtendScript_LogFile();
myLogFile.log('Hey there.');
```
### Constructor options
"new" constructor takes 4 optional arguments.

```new ExtendScript_LogFile (root, logType, logDir, useDate)```

#### Argument 1 : root
is an alternate root object to tack on a 'logFile' alias
By passing $.global as first arg, we get global log and console objects!

```
root = $.global;// root to add convenience aliases to

var myExplicitLogFileVariable = new ExtendScript_LogFile(root);
myExplicitLogFileVariable.log('So explicit!');// call from a var
logFile.log('Like magic.');// uses the $.global.logFile we made
```

#### Argument 2 : logType
 specifies a non-"default" *type* and makes a file name
```
var myLogFile = new ExtendScript_LogFile();
myLogFile.log('Hey there.');

var specialLogFile = new ExtendScript_LogFile(null,"special");
specialLogFile.log('Salutations.');

// prints to:
// ./logs/default_2021-05-28T16-15-37.611.log >>[2021-05-28T16:15:37.612] default : Hey there.
// ./logs/special_2021-05-28T16-15-37.656.log >>[2021-05-28T16:15:37.657] special : Salutations.
```

#### Argument 3 : logDir (default: './logs/')
a non-"default" directory path to save the log to
```
root = $.global;// root to add convenience aliases to
logType = "special";// name other than "default"
logDir = '~/Desktop/mylogcabin/';// custom log directory

var myLogFile = new ExtendScript_LogFile(root, logType, logDir);
myLogFile.log('Salutations.');

// prints to:
// ~/Desktop/mylogcabin/special_2021-05-28T16-15-37.656.log >>[2021-05-28T16:15:37.657] special : Salutations.
```

#### Argument 3 : useDate (default: true)
specifies if the date should be prepended to the log entries
can be changed with `.useDate(false)`
```
root = $.global;// root to add convenience aliases to
logType = "special";// name other than "default"
logDir = '~/Desktop/mylogcabin/';// custom log directory
useDate = false;

var myLogFile = new ExtendScript_LogFile(root, logType, logDir, useDate);

myLogFile.log('Salutations.');

// prints to:
// ~/Desktop/mylogcabin/special_2021-05-28T16-15-37.656.log >> special : Salutations.
```

## Use the log file
`.log()` and `.writeln()` do the same thing...

`.useDate(false)` will disable the date printing in each entry

### Attatch to namespace or other log object
```
myLogFile = new ExtendScript_Log($.global);
logFile.log('Messages are good.');

var namespace = {};// maybe some other log system?

myLogSafeFile = new ExtendScript_Log(namespace);
namespace.logFile.log('This is way safer.');
```
## Cleanup
You can `.clear()` the contents or `.remove()` the file from disk.

You can also clear out of the same *type* with `.removeOld()` for non-current or `.removeAll()` for all.
```
myLogFile = new ExtendScript_Log();
logfile.log('Messages are good.');

myLogSafeFile = new ExtendScript_Log(namespace,'special');
namespace.logFile.log('This is way safer.');

myLogSafeFile.remove();

// make a bunch of "default" logs...
myLogFile = new ExtendScript_Log();
myLogFile = new ExtendScript_Log();
myLogFile = new ExtendScript_Log();
myLogFile = new ExtendScript_Log();

// purge all but latest "default" log.
myLogFile.removeOld();

// now only the current "default" file is left
myLogFile.removeAll();

myLogSafeFile = new ExtendScript_Log(namespace);

```

# Bonus Features
## Logs for unsaved scripts:
Tries to make a log file in ./logs in the location of the currently running
script, but will fall back to ~/Desktop/ExtendScript_Log_UnsavedScripts/

## Compatible with ExtendScript_Log:
This is used as a optional extra in [ExtendScript_Log](https://github.com/MaxJohnson/extendscript_log)

Like peanutbutter and chocolate...