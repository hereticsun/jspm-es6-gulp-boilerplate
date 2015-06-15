Front End Readme
=====================================

### Prerequisite's

- You must install n `$ npm install -g n`
- This project uses n v0.12.0 `$ n 0.12.0`
- This project uses npm v2.5.1 `$ npm install npm -g.`
- This project uses JSPM `$ npm install jspm -g`

---

### Getting up and running

1. Navigate to the root Front End Directory
2. Run `npm install` to install the npm modules
3. Run `jspm install` to install the jspm modules
4. Run `gulp dev` (may require installing Gulp globally `npm install gulp -g`)
5. Your browser will automatically be opened and directed to the browser-sync proxy address
6. To prepare assets for production, run the `gulp prod` task (Note: the production task does not fire up the express server, and won't provide you with browser-sync's live reloading. Simply use `gulp serve` to manually start a server)

With `gulp` running, the server is up and serving files from the `/dest` directory. Any changes in the `/src` directory will be automatically processed by Gulp and the changes will be injected to any open browsers pointed at the proxy address.

---

This setup uses the latest versions of the following libraries:

- [SASS](http://sass-lang.com/)
- [Gulp](http://gulpjs.com/)
- [jspm](http://jspm.io/)

Along with many Gulp libraries (these can be seen in either `package.json`, or at the top of each task in `/gulp/tasks/`).

---

### SASS

SASS, standing for 'Syntactically Awesome Style Sheets', is a CSS extension language adding things like extending, variables, and mixins to the language. A Gulp task (discussed later) is provided for compilation and minification of the stylesheets based on this file.

---

### jspm with SystemJS and Babel

- jspm is a package manager for the SystemJS universal module loader, built on top of the dynamic ES6 module loader
- Load any module format (ES6, AMD, CommonJS and globals) directly from any registry such as npm and GitHub with flat versioned dependency management. Any custom registry endpoints can be created through the Registry API.
- For development, load modules as separate files with ES6 and plugins compiled in the browser.
- For production (or development too), optimize into a bundle, layered bundles or a self-executing bundle with a single command.

---

### Gulp

Gulp is a "streaming build system", providing a very fast and efficient method for running your build tasks.

##### Web Server

Gulp is used here to provide a very basic node/Express web server for viewing and testing your application as you build. It serves static files from the `build/` directory, leaving routing up to Django/AngularJS. All Gulp tasks are configured to automatically reload the server upon file changes. The application is served to `localhost:3000` once you run the `gulp` task. To take advantage of the fast live reload injection provided by browser-sync, you must load the site at the proxy address (which usually defaults to `server port + 1`, and within this boilerplate will by default be `localhost:3001`.)

##### Scripts

A number of build processes are automatically run on all of our Javascript files:

- **JSHint:** Gulp is currently configured to run a JSHint task before processing any Javascript files. This will show any errors in your code in the console, but will not prevent compilation or minification from occurring.
- **SystemJS Builder:** Provides a single-file build for SystemJS of mixed-dependency module trees. Builds ES6 into ES5, CommonJS, AMD and globals into a single file in a way that supports the CSP SystemJS loader as well as circular references.
- **Inline JS:** For any scripts that need to occur before the DOM or before the main build JS file has loaded there are two JS files that are generated `header.js` and `footer.js` and automatically injected into the index.html.

When `gulp prod` is run the resulting file (`app.js`) is placed inside the directory `/dest/js/`.



##### Styles

Just one task is necessary for processing our SASS files, and that is `gulp-sass`. This will read the `styles.scss` file, processing and importing any dependencies and then minifying the result. This file (`styles.css`) is placed inside the directory `/dest/css/`. There is also the `ie.scss` file which is for IE8 and below, the only difference being it has the media queries removed.

- **gulp-autoprefixer:** Gulp is currently configured to run autoprefixer after compiling the scss.  Autoprefixer will use the data based on current browser popularity and property support to apply prefixes for you. Autoprefixer is recommended by Google and used in Twitter, WordPress, Bootstrap and CodePen.

- **gulp-combine-media-queries:** Combine matching media queries into one media query definition. Useful for CSS generated by preprocessors using nested media queries.

##### Images

Any images placed within `/src/assets/img` will be automatically copied to the `dest/assets/img` directory. If running `gulp prod`, they will also be compressed via imagemin.

##### Watching files

All of the Gulp processes mentioned above are run automatically when any of the corresponding files in the `/src` directory are changed, and this is thanks to our Gulp watch tasks. Running `gulp dev` will begin watching all of these files, while also serving to `localhost:3000`, and with browser-sync proxy running at `localhost:3001` (by default).

##### Production Task

Just as there is the `gulp` task for development, there is also a `gulp prod` task for putting your project into a production-ready state. This will run each of the tasks, while also adding the image minification task discussed above. There is also an empty `gulp deploy` task that is included when running the production task. This deploy task can be fleshed out to automatically push your production-ready site to your hosting setup.

**Reminder:** When running the production task, gulp will not fire up the express server and serve your index.html. This task is designed to be run before the `deploy` step that may copy the files from `/dest` to a production web server.


##### Generating Documentation

This project uses jsdoc to automatically generate Documentation via `gulp prod`. See http://usejsdoc.org/ for more information. Use `gulp doc` to generate the docs manually.

##### Inspiration and Helpful Links

- http://fdietz.github.io/2015/04/13/day-1-how-to-build-your-own-team-chat-in-five-days.html
- http://developer.telerik.com/featured/choose-es6-modules-today/
- http://martinmicunda.com/2015/02/09/how-to-start-writing-apps-with-es6-angular-1x-and-jspm/
- https://github.com/Workiva/karma-jspm
- http://teropa.info/blog/2014/10/24/how-ive-improved-my-angular-apps-by-banning-ng-controller.html