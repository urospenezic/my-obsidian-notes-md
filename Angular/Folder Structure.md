https://angular.dev/style-guide

Keep the app folder minimal

Generate layout folder

Organize code by features (generate features folder where feature components live)

generate a core folder -> for services

folder called shared -> shared stuff between components

**angular.json file, near the top is "schmatics" section. we can add schematics for generating components etc. angular intellisense is useful here because there's a shit ton of stuff. But, for example, if we don't wanna run --skip-tests every time in the CLI we can do:

`"schematics": {`
        `"@schematics/angular:component": {`
          `"skipTests": true,`
          `"path": "src"`
        `}`
      `},`

also for placing services always into services folder:

`"@schematics/angular:service": {`
          `"skipTests": true,`
          `"path": "src/core/services"`
        `}`

