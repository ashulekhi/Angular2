# Angular2

Creating your first Angular 2 App – Detailed Step by Step tutorial
The goal of this post is to create a simple Angular 2 application in 8 steps (using TypeScript, on a windows machine).

Step 1: Install Node.js
Node.js is a prerequisite for developing Angular 2 apps in TypeScript. Install Node.js® and npm if they are not already installed on your machine.

Why we need Node.js and npm to create Angular 2 Apps?

Technically, Node.js and NPM are not needed to create Angular 2 apps. It does ease things though. Here are the main reasons behind this choice:

TypeScript: Since we are going to develop our application using TypeScript, we need to transpile our .ts files to get them into .js, which can be done on-the-fly easily with Node.js and NPM.
Web Server: Having Node.js helps in serving your Angular SPA from a “real” albeit light web server.
Download Packages: NPM is a packet manager that allows us to download libraries and packages to use in Angular 2. Just type npm install in the project folder to install dependencies and get our angular project going.
What is transpile? How is it different from Compile?

Transpiling is a specific kind of compiling. Compiling is the general term for taking source code written in one language and transforming into another. Transpiling is a specific term for taking source code written in one language and transforming into another language that has a similar level of abstraction.


 
So when you compile C#, your method bodies are transformed by the compiler into IL. This cannot be called transpiling because the two languages are very different levels of abstraction.

When you compile TypeScript, it is transformed by the compiler into JavaScript. These are very similar levels of abstraction, so you could call this transpiling.

Back to creating our first Angular 2 application, install Node.js® and npm if they are not already installed on your machine.

Step 2: Create project folder & add configuration files
The next step is to create a project folder. Go to your windows explorer and create a folder for your project. I’m going to name it ‘Angular2App‘.

Once the folder is created, add the following package definition and configuration files to it. All Angular 2 apps that are developed using TypeScript would need these files.

Just open a notepad and save it with the names given below:

package.json : Lists packages that our app will depend on and defines some useful scripts.
tsconfig.json : Is a TypeScript compiler configuration file
typings.json : Identifies TypeScript definition files
systemjs.config.js : The SystemJS configuration file.
Let’s try to understand what those four files are and why we need them

package.json

A package.json file contains meta data about our app. Most importantly, it includes the list of dependencies to install from npm when running npm install.

tsconfig.json

As we have learnt earlier, we need to transpile our .ts files to .js files and tsconfig.json is a TypeScript configuration file that guides the compiler as it generates JavaScript files.

typings.json

Many JavaScript libraries such as jQuery, the Jasmine testing library, and Angular itself, extend the JavaScript environment with features and syntax that the TypeScript compiler doesn’t recognize natively. When the compiler doesn’t recognize something, it throws an error. We use TypeScript type definition files — d.ts files — to tell the compiler about the libraries we load.

systemjs.config.js

We use SystemJS to load application and library modules. There are alternatives that work just fine including the well-regarded webpack. SystemJS happens to be a good choice. All module loaders require configuration and this is what we use this file for.

Now that we understand what those files are, lets add content into them.

package.json

{
  "name": "Angular2App",
  "version": "1.0.0",
  "scripts": {
    "start": "tsc && concurrently \"npm run tsc:w\" \"npm run lite\" ",
    "lite": "lite-server",
    "postinstall": "typings install",
    "tsc": "tsc",
    "tsc:w": "tsc -w",
    "typings": "typings"
  },
  "license": "ISC",
  "dependencies": {
    "@angular/common": "2.0.0-rc.4",
    "@angular/compiler": "2.0.0-rc.4",
    "@angular/core": "2.0.0-rc.4",
    "@angular/forms": "0.2.0",
    "@angular/http": "2.0.0-rc.4",
    "@angular/platform-browser": "2.0.0-rc.4",
    "@angular/platform-browser-dynamic": "2.0.0-rc.4",
    "@angular/router": "3.0.0-beta.1",
    "@angular/router-deprecated": "2.0.0-rc.2",
    "@angular/upgrade": "2.0.0-rc.4",
    "systemjs": "0.19.27",
    "core-js": "^2.4.0",
    "reflect-metadata": "^0.1.3",
    "rxjs": "5.0.0-beta.6",
    "zone.js": "^0.6.12",
    "angular2-in-memory-web-api": "0.0.14",
    "bootstrap": "^3.3.6"
  },
  "devDependencies": {
    "concurrently": "^2.0.0",
    "lite-server": "^2.2.0",
    "typescript": "^1.8.10",
    "typings":"^1.0.4"
  }
}
tsconfig.json


{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "moduleResolution": "node",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  }
}
typings.json

{
  "globalDependencies": {
    "core-js": "registry:dt/core-js#0.0.0+20160602141332",
    "jasmine": "registry:dt/jasmine#2.2.0+20160621224255",
    "node": "registry:dt/node#6.0.0+20160621231320"
  }
}
systemjs.config.js


/**
 * System configuration for Angular 2 samples
 * Adjust as necessary for your application needs.
 */
(function(global) {
  // map tells the System loader where to look for things
  var map = {
    'app':                        'app', // 'dist',
    '@angular':                   'node_modules/@angular',
    'angular2-in-memory-web-api': 'node_modules/angular2-in-memory-web-api',
    'rxjs':                       'node_modules/rxjs'
  };
  // packages tells the System loader how to load when no filename and/or no extension
  var packages = {
    'app':                        { main: 'main.js',  defaultExtension: 'js' },
    'rxjs':                       { defaultExtension: 'js' },
    'angular2-in-memory-web-api': { main: 'index.js', defaultExtension: 'js' },
  };
  var ngPackageNames = [
    'common',
    'compiler',
    'core',
    'forms',
    'http',
    'platform-browser',
    'platform-browser-dynamic',
    'router',
    'router-deprecated',
    'upgrade',
  ];
  // Individual files (~300 requests):
  function packIndex(pkgName) {
    packages['@angular/'+pkgName] = { main: 'index.js', defaultExtension: 'js' };
  }
  // Bundled (~40 requests):
  function packUmd(pkgName) {
    packages['@angular/'+pkgName] = { main: '/bundles/' + pkgName + '.umd.js', defaultExtension: 'js' };
  }
  // Most environments should use UMD; some (Karma) need the individual index files
  var setPackageConfig = System.packageWithIndex ? packIndex : packUmd;
  // Add package entries for angular packages
  ngPackageNames.forEach(setPackageConfig);
  var config = {
    map: map,
    packages: packages
  };
  System.config(config);
})(this);
We shall learn more about the content of those files in a later post. For now just add the above content and save them. Your project folder should like this:

angular2-project-folder

Step 3: Install packages
We install the packages listed in package.json using npm. From programs, go to Node.js and open the command window. Next, navigate to your project folder.

Enter the following command:

1
npm install
The installation might take a few minutes and you should finally see the screen like below. All is well if there are no console messages starting with npm ERR! at the end of npm install. There might be a few npm WARN messages along the way — and that is perfectly fine.

angular2-tut-1
angular2-tut-2

With this, you should have all the dependencies installed. You project folder will now have additional folders for node_modules and typings as below:

angular2-project-folder1

With this, we are all set. The next step is to write some code.

Step 4: Add Angular Component
Before we start writing our first Angular component, lets organize a bit and create an app folder within our project folder.

Within the app folder create a file called app.component.ts. This is where we will be writing our first component. Open the file and add the following content to it.


import { Component } from '@angular/core';
@Component({
  selector: 'my-app',
  template: '<h1>My First Angular 2 App</h1>'
})
export class AppComponent { }
Every Angular 2 app has at least one root component, conventionally named AppComponent, that hosts the client user experience. Components are the basic building blocks of Angular applications. A component controls a portion of the screen — a view — through its associated template.

The above code is  extremely simple but, it has the essential structure of every component we’ll ever write. Lets understand each of the lines in detail.

import

1
import { Component } from '@angular/core';
Angular apps are modular. They consist of many files each dedicated to a purpose. Angular itself is modular. It is a collection of library modules each made up of several, related features that we’ll use to build our application.

When we need something from a module or library, we import it. Here we import the Angular 2 core so that our component code can have access to the @Component decorator.

What is @Component decorator?

Component is a decorator function that takes a metadata object as argument. We apply this function to the component class by prefixing the function with the @ symbol and invoking it with a metadata object, just above the class.

@Component is a decorator that allows us to associate metadata with the component class. The metadata tells Angular how to create and use this component.

@Component({
  selector: 'my-app',
  template: '<h1>My First Angular 2 App</h1>'
})
This particular metadata object has two fields, a selector and a template.

The selector specifies a simple CSS selector for an HTML element that represents the component. The element for this component is named my-app. Angular creates and displays an instance of our AppComponent wherever it encounters a my-app element in the host HTML.

The template specifies the component’s companion template, written in an enhanced form of HTML that tells Angular how to render this component’s view.

Our template is a single line of HTML announcing “My First Angular 2 App“.

A more advanced template could contain data bindings to component properties and might identify other application components which have their own templates. These templates might identify yet other components. In this way an Angular application becomes a tree of components.

Component class

1
export class AppComponent { }
At the bottom of the file is an empty, do-nothing class named AppComponent. When we’re ready to build a substantive application, we can expand this class with properties and application logic.

We export AppComponent so that we can import it elsewhere in our application, as we’ll see when we create main.ts.

Step 5: Add main.ts
Now we need something to tell Angular to load the root component. Create the file app/main.ts with the following content:

1
2
3
import { bootstrap }    from '@angular/platform-browser-dynamic';
import { AppComponent } from './app.component';
bootstrap(AppComponent);
We import the two things we need to launch the application:

Angular’s browser bootstrap function
The application root component, AppComponent.
Then we call bootstrap with AppComponent.

Notice that we import the bootstrap function from @angular/platform-browser-dynamic, not@angular/core. Bootstrapping isn’t core because there isn’t a single way to bootstrap the app. True, most applications that run in a browser call the bootstrap function from this library. But it is possible to load a component in a different environment. We might load it on a mobile device with Apache Cordova or NativeScript. These targets require a different kind of bootstrap function that we’d import from a different library.

Step 6: Add index.html
In the project root folder (not the app folder), create an index.html file and paste the following lines into it:
<html>
  <head>
    <title>Angular2App</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="styles.css">
    <!-- 1. Load libraries -->
     <!-- Polyfill(s) for older browsers -->
    <script src="node_modules/core-js/client/shim.min.js"></script>
    <script src="node_modules/zone.js/dist/zone.js"></script>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <!-- 2. Configure SystemJS -->
    <script src="systemjs.config.js"></script>
    <script>
      System.import('app').catch(function(err){ console.error(err); });
    </script>
  </head>
  <!-- 3. Display the application -->
  <body>
    <my-app>Loading...</my-app>
  </body>
</html>
There are three sections in the above HTML that are noteworthy:

The JavaScript libraries
Configuration file for SystemJS, and a script where we import and run the app module which refers to the main file that we just wrote.
The <my-app> tag in the <body> which is where our app lives!
JavaScript libraries

     <!-- Polyfill(s) for older browsers -->
    <script src="node_modules/core-js/client/shim.min.js"></script>
 
    <script src="node_modules/zone.js/dist/zone.js"></script>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
The first library that we included is core-js‘s ES2015/ES6 shim which monkey patches the global context (window) with essential features of ES2015 (ES6).

Next are the polyfills for Angular2, zone.js and reflect-metadata. Polyfills are required for Angular 2 to function properly (the exact list depends on the browser used) and external dependencies ([zone.js]

A Zone is an execution context that persists across async tasks. Zones are an idea borrowed from Dart that Angular 2 uses to efficiently know when to update the view. We shall explore more about it in the later posts.

reflect-metadata is used to enable dependency injection through decorators.

System.js  is a library written by Guy Bedford (and others) built upon es6-module-loader to provide a way to load not only ES6 modules, but also CommonJS, AMD and global scripts. It adds a global object called System.

Configure SystemJS

  <!-- 2. Configure SystemJS -->
    <script src="systemjs.config.js"></script>
    <script>
      System.import('app').catch(function(err){ console.error(err); });
    </script>
Earlier we added systemjs.config.js file to the project root. SystemJS is not the only module loader that will work with Angular 2. Other module loaders, such as WebPack, can be swapped in instead.

So, via this configuration, we create a map to tell SystemJS where to look when we import some module. Then, we register all our packages to SystemJS: all the project dependencies and our application package, app. The app package tells SystemJS what to do when it sees a request for a module from the app/ folder.

Our code above makes such requests when one of its application TypeScript files has an import statement like this:

1
import { AppComponent } from './app.component';
Notice that the module name (after from) does not mention a filename extension. In the configuration we tell SystemJS to default the extension to js, a JavaScript file.

 <script>
      System.import('app').catch(function(err){ console.error(err); });
 </script>
In the code above, the System.import call tells SystemJS to import the main file (main.js … after transpiling main.ts, remember?); main is where we tell Angular to launch the application. We also catch and log launch errors to the console.

All other modules are loaded upon request either by an import statement or by Angular itself.

<my-app>


 <body>
    <my-app>Loading...</my-app>
  </body>
When Angular calls the bootstrap function in main.ts, it reads the AppComponent metadata, finds the my-app selector, locates an element tag named my-app, and renders our application’s view between those tags.

Now that we have out index file ready, lets add some styles to it.

Step 7: Add styles.css
Create a styles.css file in the project root folder and add some minimal styles for this example as shown below:

h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
body {
  margin: 2em;
}
 
 /*
  * See https://github.com/angular/angular.io/blob/master/public/docs/_examples/styles.css
  * for the full set of master styles used by the documentation samples
  */
Step 8: Build and Run
We are all done now. The final step is to build and run the app. For this, go to the Node.js command prompt and assuming you are in the project root folder type the following command :

1
npm start
That command runs two parallel node processes

The TypeScript compiler in watch mode
A static server called lite-server that loads index.html in a browser and refreshes the browser when application files change
Once we run the above command, our application gets built and opens up index.html in a browser. Following is what you should see:

angular2-tut-3
angular2-tut-4

In the browser:

angular2-tut-5

Now change the template in your component and you should see changes automatically reflected in your browser.

The final structure of your project folder should look like below:

angular2-tut-6

This post is inspired from Angular2’s Official quick Start Guide. I have added more detail so someone who is new to Node.js and npm can understand it better.
