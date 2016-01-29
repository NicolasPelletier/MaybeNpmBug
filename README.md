# MaybeNpmBug
This repository contains an example demonstrating a bug with npm 3.x.x

##What is the bug
My main application here has a devDependency to 'oauth 0.9.14', and
the pack1 node modules I am trying to install has a dependency to oauth 0.9.10.

Technically, both versions should be downloaded and one should be installed
inside my pack1 node modules.

##Main app - package.json
```
{
  "name": "mainApp",
  "version": "1.0.0",
  "description": "Dummy main app to demonstrate a bug",
  "main": "index.js",
  "author": "",
  "license": "MIT",
  "dependencies": {
    "pack1": "./pack1"
  },
  "devDependencies": {
    "oauth": "0.9.14"
  }
}
```

## 'pack1' node module - package.json
```
{
  "name": "pack1",
  "version": "0.1.2",
  "description": "Dummy module to demonstrate a bug",
  "license": "MIT",
  "dependencies": {
    "oauth": "0.9.10"
  }
}
```

##With npm 3.x.x
However, when I run npm install, I get:
```bash
mainApp@1.0.0 /home/pelletn/github/MaybeNpmBug
├── oauth@0.9.14 
└── pack1@0.1.2 

$ npm -v && node -v
3.6.0
v0.12.9

$ npm ls
mainApp@1.0.0 /home/pelletn/github/MaybeNpmBug
├── oauth@0.9.14
└─┬ pack1@0.1.2
  └── UNMET DEPENDENCY oauth@0.9.10

npm ERR! missing: oauth@0.9.10, required by pack1@0.1.2
```


##With npm 2.14.16
When I run an older version of NPM, I get:
```bash
auth@0.9.14 node_modules/oauth

pack1@0.1.2 node_modules/pack1
└── oauth@0.9.10

$ npm -v && node -v
2.14.16
v0.12.9

$ npm ls
mainApp@1.0.0 /home/pelletn/github/MaybeNpmBug
├── oauth@0.9.14
└─┬ pack1@0.1.2
  └── oauth@0.9.10
```

##The Bug
As you can see, version 3.x.x does not download oauth 0.9.10 but older version of
npm do.

## How to reproduce the bug
Clone this repository and run ```npm install``` to demonstrate that
only one copy of oauth is dowloaded instead of two.

##What is not a bug
If the oauth 0.9.14 of the main repo is installed as a dependency, then it works. It only fails when it is a devDependency.
