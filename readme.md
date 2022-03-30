# Laravel Electron
Making Laravel desktop application using Electron Js.
<p>
    Electron Js is very much popular for building a cross-platform desktop application. If you are Laravel
developer and you are wanted to convert your existing Laravel application into a desktop application
or build a new one using Electron Js, then it's for you. Here I'm going to show you a step by step
process for making Laravel desktop application using Electron Js. Let's know a little bit about
Electron Js if you are new in Electron Js.

Electron Js
Electron Js is an open-source framework for building a cross-platform desktop application which is
developed and maintained by GitHub. With the help of Electron Js, you can easily make a desktop
application using HTML, CSS and JavaScript!
Build a Laravel Desktop application using Electron Js

Laravel desktop application using Electron Js | AIFX Knowledgebase

1. Make a directory for the project & initialize the project.
2. Install npm dependencies
3. Make the required directories.
4. Main js setup.
5. Test the application.

1. Make a directory for the project & initialize the project
First, we need to make a directory for our project for an example laravel-electron. After making the
directory we have to initialize the node project using the command given below. Keep in mind before
doing that, make sure you have installed node js into your machine.

`npm init -y`

2. Install npm dependencies
First, we need to install Electron Js as a dev dependency to our project. To build distribute a version
of our desktop application we need electron-packager also. For that run the command given below.

`npm i --save-dev electron electron-packager` 

Now install node-php-server npm package for PHP server.

`npm i --save node-php-server`

3. Make required directories
We need two directories. Create two directories inside our project directory name with <b>php</b> and
<b>www</b>. Inside <b>php</b> directory, we'll keep our required php version executable files according to our
Laravel application need and inside <b>www</b> directory, we will keep our entire Laravel application.

4. Main Js setup
Now the main part to run the desktop application with our Laravel application. To do that, make a
main.js file inside our project root directory and add properties to package.json file.

```
"main": "main.js",
"scripts": {
 "start": "electron ."
}
```



Now open the main.js file and code for setup php server and run our laravel application as a desktop
application.
```
const electron = require('electron')
const path = require('path')

const BrowserWindow = electron.BrowserWindow
const app = electron.app

app.on('ready', () => {
  createWindow()
})

var phpServer = require('node-php-server');
const port = 8000, host = '127.0.0.1';
const serverUrl = `http://${host}:${port}`;


let mainWindow

function createWindow() {
  // Create a PHP Server
  phpServer.createServer({
    port: port,
    hostname: host,
    base: `${__dirname}/www/public`,
    keepalive: false,
    open: false,
    bin: `${__dirname}/php/php.exe`,
    router: __dirname + '/www/server.php'
  });
  
  // Create the browser window.
  const {
    width,
    height
  } = electron.screen.getPrimaryDisplay().workAreaSize
  mainWindow = new BrowserWindow({
    width: width,
    height: height,
    show: false,
    autoHideMenuBar: true
  })

  mainWindow.loadURL(serverUrl)

  mainWindow.webContents.once('dom-ready', function () {
    mainWindow.show()
    mainWindow.maximize();
    // mainWindow.webContents.openDevTools()
  });

  

  // Emitted when the window is closed.
  mainWindow.on('closed', function () {
    phpServer.close();
    mainWindow = null;
  })
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
//app.on('ready', createWindow) // <== this is extra so commented, enabling this can show 2 windows..

// Quit when all windows are closed.
app.on('window-all-closed', function () {
  // On OS X it is common for applications and their menu bar
  // to stay active until the user quits explicitly with Cmd + Q
  if (process.platform !== 'darwin') {
    // PHP SERVER QUIT
    phpServer.close();
    app.quit();
  }
})

app.on('activate', function () {
  // On OS X it's common to re-create a window in the app when the
  // dock icon is clicked and there are no other windows open.
  if (mainWindow === null) {
    createWindow()
  }
})
```

5. Test the application
We just configured all our required steps. Now put your Laravel application into the www directory
and run the command given below for test the Laravel application is running with Electron js.

`npm start`

Packaging the application
After the end of the development, we can distribute our final application by packaging the application
using electron packager which we have installed as dev-dependecy earlier. To make distribute a
version of your application include these into your package.json file at scripts section.
```
"scripts": {
    "start": "electron .",
    "build":"electron-packager . LaravelElectron --out=dist"
}
```
Now if we run `npm run build` then it'll make a distribute version inside a dist directory of our project.
So that, we can share that version with anyone to use our Laravel application as a desktop
application.

Repository Laravel Electron desktop application:

I have described a details and step by step process for making a laravel desktop application using
electron js. If you want to use the boilerplate of Laravel Electron js to build Laravel desktop
application then visit the Laravel Electron GitHub repository.

1. Download Laravel Electron repository
2. Run command `npm install`
3. Put your laravel application into <b>www</b> directory (currently no project inside)
4. Run application by command `npm start`.

Current PHP V 8.0.17 
If you decide to change the version remember to include/add your php.ini file
`extension=php_openssl.dll` was my first and only error - but you might need to revise your extensions per project.

I hope this step by step tutorial for making Laravel desktop application using Electron Js will help
you to make your own Laravel desktop application. If this post is helpful to you then please share it
with others so that they can be helped.

</p>

### Check the complete tutorial and usage on [discord](https://discord.gg/Pqd68PMYK3)
