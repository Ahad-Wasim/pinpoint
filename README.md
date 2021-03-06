# Pinpoint

  A mobile app that shows real-time geolocation of people with interests in common with you, and shows you their most popular hangout spots.

## Development Team

  * [Justin Shaw](https://github.com/jshaw22)
  * [Ahad Wasim](https://github.com/Ahad-Wasim)
  * [Gloria Ma](https://github.com/gloriama) (Scrum Master)
  * [Wayne Chaw](https://github.com/waynechaw) (Product Owner)

## Table of Contents

  1. [Features](#features)
  2. [Architecture Overview](#architecture-overview)
    1. [Tech Stack](#tech-stack)
    2. [System Architecture](#system-architecture)
    3. [Database Schemas](#database-schemas)
  3. [Contributing](#contributing)
    1. [Setting up XCode](#setting-up-xcode)
    2. [Setting up Postgres](#setting-up-postgres)
  4. [Useful Commands](#useful-commands)

## Features

  Log in and immediately see where people are!

  * **Live map** of where people with tags in common with you are currently located
  * **Filtered display** of users based on any specific tag
  * **Hotspot locations** for any specific tag, based on how frequently people have visited them
  * **Visit statistics** for any location on the globe, grouped by day of the week
  * **Viewable tags** for each live user, to see what they're interested in

![Live map, viewable tags, and filtered display](https://giant.gfycat.com/MisguidedFrightenedAsiaticgreaterfreshwaterclam.gif) ![Hotspot locations and visit statistics](https://fat.gfycat.com/AdmiredLargeJumpingbean.gif)

## Architecture Overview

### Tech Stack
  
  The front-end is written in React Native and Redux, and the back-end uses Node/Express and a PostgreSQL database, with Sequelize as the ORM. Socket.IO is also used extensively to pass location updates between the client and server.

### System Architecture

![System architecture](http://s31.postimg.org/apinlbqbf/pinpoint.png)

### Database Schemas

![Database schemas](http://i.imgur.com/oxvTk9f.png)

  There are three main entities in the database: **users**, **tags**, and **visits**. We value your privacy, so **we do not save any identifying information with where you've been**. Join tables help attach a user's tags to each of their visits, but no visit is ever identified specifically with any user.

## Contributing

  If you'd like to contribute to the repo, here are all the steps you need to get started.

### Setting up XCode

  This will allow you to render the front-end on your computer or iPhone.
  
  1. If you don't already have it, download and install [XCode 7.2](https://itunes.apple.com/us/app/xcode/id497799835). This is a large (4GB+) program available free from the App Store, used to run the simulator for React Native.
    * Make sure you have Mac OSX 10.10.5 or later.
    * Different XCode versions may also work, but download at your own discretion.
  2. Install global dependencies for React Native: `sudo npm install -g react-native react-native-cli rnpm`
  3. Clone this repo: `git clone https://github.com/triangulating-rhombus/pinpoint.git`
  4. Install general dependencies: `cd pinpoint` and `npm install`
  5. Install client dependencies: `cd pinpoint` (again) and `npm install` (again)
  6. Link React Native dependencies: `rnpm link`
  7. Replace the file located at node_modules/react-native/Libraries/Animated/src/AnimatedImplementation.js with the code [here](https://gist.githubusercontent.com/lelandrichardson/c0d938e02301f9294465/raw/5053cebc66989d27697bbb08450f360555309b0c/AnimatedImplementation.js).
  8. Open the project in XCode: `open ios/pinpoint.xcodeproj`. This will automatically build the project, which may take a minute or two.
  9. When you see 'Build Succeeded', click the Play button at the top left of the XCode window. This will automatically run a server, which will appear in a terminal window, and (after a minute or so) the iPhone simulator.
  10. *(Optional: running the project on your iPhone)* You can also provision the project to run on your iPhone. From the XCode window, locate the AppDelegate.m file in the pinpoint file directory. Comment out the `jsCodeLocation` line in Option 1 and uncomment `jsCodeLocation` in Option 2, and then save the file. Remember to change this back if you want to use the XCode simulator again!

### Setting up Postgres
  
  1. If you don't already have Postgres, install it: `brew install postgres`
  2. In the terminal, run:

        > initdb pinpointdb 
        > pg_ctl -D pinpointdb -l logfile start
        > createdb pinpointdb

  3. Run the psql command line tool to create a role:

        > psql pinpointdb
        $ CREATE ROLE postgres WITH superuser;
        $ ALTER ROLE postgres WITH login;
        $ \q

  4. Make sure you are in the root directory of the repo and then start the server: `npm start`

## Useful Commands

  We've compiled a list of helpful commands to help beginning developers learn their way around Postgres and Heroku.

### Drop all database tables

  This is the easiest way to reset your database after changing schemas. **NOTE: This will permanently destroy all the data stored in your database.**
```
  > psql pinpointdb
  $ drop schema public cascade;
  $ create schema public;
  $ \q
```
  Then restart the server (type `rs` in the running server's terminal tab).

### Start your local database

```
  > pg_ctl -D pinpointdb -l logfile start
```

### Access/Update your local database via terminal

```
  > psql pinpointdb
```

### Access/Update your Heroku database via terminal

```
  > heroku pg:psql
```

### Deploy your server on Heroku

  We've deployed our server [here](http://tr-pinpoint-server.herokuapp.com), but if you make edits and want to deploy your own version, Heroku makes the process quick and easy. The free plan has its limitations, but it is sufficient for simple testing.

  1. Create an app on heroku.com. The name you choose will determine the URL of your server, e.g. http://your-app-name.herokuapp.com.

  2. From the Heroku dashboard for your app, go to Resources and provision a "Heroku Postgres Add-on". Start with the default Hobby Dev plan, which is free.

  3. Cd to your repo's root directory in terminal and add the Heroku remote:

        > heroku git:remote -a your-app-name
  
  4. To deploy code to the server, simply push to the Heroku remote:

        > git push heroku master

  (If you want to push a non-master branch, you can do so with `git push heroku your-branch-name:master`. This is not good form for a real production server, but it can be handy for testing purposes.)
