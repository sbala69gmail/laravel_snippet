### Put the below code to lunch the debuger.

```
{
    "version": "0.2.0",
    "compounds": [
        {
            "name": "Launch & Debug",
            "configurations": [ "Launch Program", "Launch localhost" ]
        }
],
"configurations": [
    {
        "type": "php",
        "request": "launch",
        "name": "Launch Program",
        "cwd": "${workspaceRoot}",
        "port": 9000
    },
    {
        "name": "Launch localhost",
            "type": "edge",
            "request": "launch",
            "url": "http://localhost/public",
            "webRoot": "${workspaceRoot}"
        }
    ]
}
```
