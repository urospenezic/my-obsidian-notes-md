Debugger is accessible through the main toolbar where git, file explorer and extension tabs are.

We gotta set it up first, click on launch.json file creation. Select .NET, debugger for JS doesn't rly make much sense. after that select .NET. Add configuration at the bottom once it created the file -> add launch program. End result looks like this:

`{`
    `// Use IntelliSense to learn about possible attributes.`
    `// Hover to view descriptions of existing attributes.`
    `// For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387`
    `"version": "0.2.0",`
    `"configurations":` 
        `{`
            `"name": "C#: API Debug",`
            `"type": "dotnet",`
            `"request": "launch",`
            `"projectPath": "${workspaceFolder}/API/API.csproj"`
                `},`
        `{`
            `"name": ".NET Core Attach",`
            `"type": "coreclr",`
            `"request": "attach"`
        `}`
    `]`
`}`


