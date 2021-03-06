# Angular2-Meteor [![bitHound Overall Score](https://www.bithound.io/github/Urigo/angular2-meteor/badges/score.svg)](https://www.bithound.io/github/Urigo/angular2-meteor) [![Build Status](https://travis-ci.org/Urigo/angular2-meteor.svg?branch=master)](https://travis-ci.org/Urigo/angular2-meteor)
Angular2 + Meteor integration.

##### Angular2 version: beta-6.

## Table of Contents
* [Documentation](../../#Documentation)
* [Quick Start](../../#quick-start)
    * [Install package](../../#install-package)
    * [Import Angular2 into your app](../../#import-angular2-into-your-app)
    * [Start using Meteor in your angular2 app](../../#start-using-meteor-in-your-angular2-app)
* [Demos](../../#demos)
* [Server Side](../../#server-side)
* [TypeScript Support](../../#typescript-support)
* [Roadmap](../../#roadmap)
* [Contribution](../../#contribution)

## Change Log

Check out package change log [here](CHANGELOG.md).

## Documentation
- [Tutorial](http://www.angular-meteor.com/tutorials/socially/angular2/bootstrapping)
- [Official website](http://angular-meteor.com)
- [Example application](https://github.com/Urigo/meteor-angular2.0-socially) (Final version of the tutorial)
- Questions and help - [stack-overflow `angular2-meteor` tag](http://stackoverflow.com/questions/tagged/angular2-meteor)
- [Discussions - the Meteor Forum](https://forums.meteor.com/)
- [Report issues](https://github.com/Urigo/angular2-meteor/issues)
- [Change Log, updates and breaking changes](https://github.com/Urigo/angular2-meteor/releases)
- [Meteor Blog](https://info.meteor.com/blog)
- [Official Meteor guide for best practices](http://guide.meteor.com/)
- Track Roadmap here:
- [angular2-meteor milestones](https://github.com/Urigo/angular2-meteor/milestones)
- [meteor-angular2.0-socially milestones](https://github.com/Urigo/meteor-angular2.0-socially/milestones)
- [Meteor issues related to Angular](https://github.com/meteor/meteor/labels/Project%3AAngular)

## Quick Start

### Install package:
````
    meteor add urigo:angular2-meteor
````

This package adds own HTML processor, so you'll also need to remove the standard HTML processor:

````
   meteor remove blaze-html-templates
````

For the explanation, please, read "HTML" [paragraph](http://www.angular-meteor.com/tutorials/socially/angular2/bootstrapping) in the above mentioned tutorial.

### Import Angular2 into your app:
Package supports TypeScript and Babel (.jsx file extension) as languages for development with Angular2.

ES6 modules are supported via SystemJS module loader library.

To start, create `app.ts` file, import `Component` and `View` and then bootstrap the app:
````ts
    import {Component, View} from 'angular2/core';
    import {bootstrap} from 'angular2/platform/browser';

    @Component({
      selector: 'socially'
    })
    @View({
      template: "<p>Hello World!</p>"
    })
    class Socially {}

    bootstrap(Socially);
````

Add `index.html` file to the app top folder:
````html
    <body>
       <socially></socially>
    </body>
````
At this point you should see app working and showing "Hello word".

If you have an HTML file in the app root folder with `body` or `head` tag (`index.html` in our case), the package will recognize it as your master HTML file and will skip inserting a default layout. Otherwise, it'll insert bootstrap HTML as follows:
````html
<body>
    <app></app>
</body>
````
Also, if you name your main client component `app.ts`, the package will import `client/app` System.js module for you.

You can name the component whatever you want, but when doing so you'll need to import it manually by adding the next script in the `index.html` or some JS file (say, `main.js`):
````js
Meteor.startup(() => {
    System.import('client/foo');
});
````

### Start using Meteor in your Angular2 app:
This package comes with some modules that makes it easy to use Meteor in Angular2.
To load that modules, you will need to bootsrap your app with the help of the package's bootstraper.

To do that, import `bootstrap` from Meteor-Angular2 package and remove previous one imported from `angular2/angular2`:

````ts
    import {bootstrap} from 'angular2-meteor';

    ....

    bootstrap(Socially);
````

After that, you can use Meteor collections in the same way as you would do in a regular Meteor app with Blaze.

For example, change `client/app.ts` to:
````ts
    ....

    @View({
      templateUrl: 'client/parties.html'
    })
    class Socially {
        constructor() {
          this.parties = Parties.find();
        }
    }

    ....
````

Add Angular2 template file `client/parties.html` with a content as follows:
````html
    <div *ngFor="#party of parties">
      <p>Name: {{party.name}}</p>
    </div>
````

At this moment, you are ready to create awesome apps backed by the power of Angular2 and Meteor!

## Demos

Check out two demos for the quick how-to:

* the Tutorial's [Socially app](https://github.com/Urigo/meteor-angular2.0-socially)
* [Todos](https://github.com/Urigo/Meteor-Angular2/tree/master/examples/todos) demo (deployed http://ng2-todos.meteor.com).

### Server Side
You can import TypeScript and System.js on the server side same way you can on the client side.

Similar to the client's main module `app.ts`, the package checks for the existence of the `main.ts` file in the server folder and, in case of success, will import it for you. Otherwise, you can name main module as you want and import by:
````ts
Meteor.startup(() => {
    System.import('server/foo').await();
))
````

### TypeScript Support
The package uses this TypeScript [compilers](https://github.com/barbatus/ts-compilers) to compile `.ts`-files. Please, read there how you can configure TypeScript, what options are available or how you can speed up just-in-time compilation.

TypeScript configuration file a.k.a. `tsconfig.json` is supported as well. Place a file with this name at the root folder and start adding any available TypeScript options you want. Read about the structure [here] (https://github.com/Microsoft/TypeScript/wiki/tsconfig.json). "files" property works only for the declaration files in the "typings" folder.

By default, compiler will curse on syntactic errors and will log out all missing modules and other semantic errors to the terminal.
So, if you have code like this:
````ts
    var parties = new Mongo.Collection('parties');
````
It will likely curse that `Mongo` is undefined. Luckily, the package adds Angular2 and Meteor declaration files, which means you'll need only to reference them in your TypeScript files to fix errors.

After the first run of your app, Angular2-Meteor will create a  definition file for the package API,  `typings/angular2-meteor/angular2-meteor.d.ts`, and Angular 2 definition files (since they are available only in the Angular2 NPM) in the "typings" folder. All other typings dependencies should be installed using `typigns` tool:
   
````
npm install typings -g

typings meteor --ambient
typings es6-promise --ambient
typings es6-shil --ambient
````

There are two ways to link up typings with the app:

- you can directly add a reference to, for example, `typings/angular2-meteo/angular2-meteor.d.ts` in every TypeScript file that uses  
  Angular2-Meteor and Angular 2 API as follows:

  ````ts
   /// <reference path="typings/angular2-meteor/angular2-meteor.d.ts" />
  ````
  To add Meteor API, you'll need to reference `typings/main.d.ts`, a definition that links together all other definitions installed by     `typings` tool.
- or you can add a custom TypeScript config at the root with "files"
  property set to contain `typings/angular2-meteor/angular2-meteor.d.ts` path.
  As soon as you've done this, TypeScript compiler will compiler
  every .ts-file along with `angular2-meteor.d.ts`, thus, recognizing 
  Meteor and Mongo API. See, the parties demo (example/parties) for the details.

Make sure that paths are relative to the app top folder.

The package installs typings itself but doesn't overrides existing ones in the "typings" folder. So, if you've 
updated package and started getting errors in the console, remove "angular2" folder and "angular2-meteor.d.ts" and re-start the app. New versions of them will be re-installed in the folder.

## Roadmap

This package's roadmap and its status [here](https://trello.com/b/kSa6JNCk/angular2-tutorial).

## Contribution
If you know how to make integration of Angular 2 and Meteor better, you are welcome!

For the coding style guide, we use AirBnB [rules](https://github.com/airbnb/javascript) with TypeScript specifics and max line width set to 100 symbols. Rules are partly enforced by the tslint.json file in the root (if you are not familiar with TSLint, read more [here](https://github.com/palantir/tslint)). Please, check that your code conforms with the rules before PR.

### Commit message format

This project follows the `angular` project git commit message format.
Please refer to the [official documentation](https://github.com/angular/angular.js/blob/master/CONTRIBUTING.md#-git-commit-guidelines).

### Setup repository

There is a git hook that needed to be installed manually.

```bash
git clone https://github.com/<Your Username>/angular2-meteor.git
cd angular2-meteor
ln -s ../../validate-commit-msg.js .git/hooks/commit-msg
```
