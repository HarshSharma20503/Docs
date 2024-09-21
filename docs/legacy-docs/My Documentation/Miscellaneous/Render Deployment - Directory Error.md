
_____dirname does not work in docker environment.
To overcome such use relative path like ./, etc.

```
const scriptPath = "./" + scriptName;
    const executablePath = "./" + executableName;
    let callbackExecuted = false;
    fs.writeFile(scriptPath, Code, (err) => {
        if (err) {
            if (!callbackExecuted) {
                callback({
                    success: false,
                    message: "Error While Writing!",
                    error: err
                });
                callbackExecuted = true;
            }
            return;
        }
```
---
# Tags
#render #deployment #docker #file-path-error
