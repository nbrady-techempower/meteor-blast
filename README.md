# :boom: Meteor Blast :boom:

Meteor Blast is a boilerplate designed to get you up and running with the [Meteor](https://www.meteor.com/) javascript platform and creating websites quickly. This boilerplate includes a simple chatroom example to introduce the developer to some of Meteor's design patterns.

## Getting Started

Once you have installed Meteor and cloned the project, simply run `meteor` from the command line at the root of the project.

Users are initially:
* Username: `user` Password: `password`
* Username: `admin` Password: `password`


## Packages Added

* [accounts-facebook](https://atmospherejs.com/meteor/accounts-facebook) - Easy configurable login with facebook!
* [accounts-password](https://atmospherejs.com/meteor/accounts-password) - Takes care of passwords, so you don't have to.
* [accounts-twitter](https://atmospherejs.com/meteor/accounts-twitter) - Easy configurable login with twitter!
* [poetic:materialize-scss](https://github.com/poetic/meteor-materialize-scss/) - Templating with MaterializeCSS
* [kadira:flowrouter](https://github.com/kadirahq/flow-router) - The go to router for Meteor
* [less](https://atmospherejs.com/meteor/less) - For writing less
* [mizzao:user-status](https://github.com/mizzao/meteor-user-status/) - Popular package for tracking user online status
* [pecl:loading](https://github.com/pcel/meteor-loading) - Loading screen for subscribing to lots of data.
* [percolate:migrations](https://github.com/percolatestudio/meteor-migrations/) - Support for migrations on startup

## NPM Packages
* [moment](https://www.npmjs.com/package/moment) - For handling date formatting

#### Packages removed

The two packages below ship with Meteor. And though autopublish makes it easier to develop, automatically front-loading all your data to the client, it doesn't encourage a security first approach. I'd much rather allow access as needed while building my app, than start removing things before production and possibly miss something. If you're going for ease of use and aren't dealing with sensitive data, feel free to add these packages back in to avoid the whole pub/sub model.

* autopublish
* insecure

## Files and Directory Structure

The directory structure below is adapted from common practices. You can structure your Meteor app however you'd like, though certain directory names have special rules as seen below. Load order can be important and confusing if this is your first time using Meteor. If you follow the directory structure below, and your app is fairly simple, it will probably never come up. There will be certain situations where you'll need to know what get's loaded first. Be sure to check out the documentation for more on [File Load Order](http://docs.meteor.com/#/full/fileloadorder).

#### Project root structure

```
project-root
    ├─── .meteor (for meteor packages)
    ├─── client
    ├─── imports
    ├─── lib
    ├─── private
    ├─── public
    ├─── server
    └─── tests
```

#### `client` subfolder

```
client
  ├─── config
  ├─── stylesheets
  └─── templates
      ├─── _partials
      ├─── admin
      ├─── app
      ├─── chat
      └─── home
  main.js
````

The `client` folder holds everything that will be loaded to and accessible by the client. It's important to note that anything that you wrap in `if (Meteor.isServer) { ... }` won't be accessible by client code, but could still be viewed by the client. Make sure to keep anything sensitive, like passwords, API keys, etc in folders that are only available to the server. Again, the substructure of `client` is up to you. In the case of this boilerplate, I keep all client configurations, like account.js, in `config`. All css (there are less compiler packages) and other things related to styling in `stylesheets`. I like to separate templates by `_partials`: snippets and includes used my multiple templates, `app`: templates used throughout the app, like 404.html, and then folders structured by routes.

#### `imports` subfolder

```
imports
  └─── api
      └─── messages
            ├─── client
            └─── server
            messages.js
      └─── pages
            ├─── client
            └─── server
             pages.js
      └─── users
            ├─── client
            └─── server

  └─── config
      └─── client
  └─── migrations
  └─── template_helpers
  keyboard.js
  parse.js
  roles.js
````

The `imports` folder is ignored by Meteor initially. Nothing here is loaded until explicitly directed to by the app. Starting in 1.3 Meteor is moving away from the "globals everywhere" approach that made most of it magical to begin with. Now your entire api, all your collections, should be placed here and loaded as needed by the app. If an import is made on a directory, it will try and find the `index.js` file. If you look in `/imports/api/messages/server` you'll see such a file with a few import statements. We can then make an import in our `/server/main.js` file that looks like `import '/imports/api/messages/server/';` and it will run the `index.js` file and thus the imports there. If we do things this way, when we add more files to certain directories, we can update the individual `index.js` files without having to update the import statement in our `main.js` entry points for the client and server.

#### `private` subfolder

```
private
  └─── config
```

The `private` subfolder is only available to the server. So why not just put it in the server folder? Think of the `private` folder as assets available to the server but not needed for the server to start up. Every time a client connects to the server on a new thread, it spins up the server loading process. This is a good place to hold large files the server may at some point, or settings and configuration files. If you're sharing your project on github, make sure to add files with sensitive information to your .gitignore. You don't want to publish your secret keys.

#### `public` subfolder

```
public
  └─── images
```

The `public` subfolder is available to the server and the client, but acts like the `private` subfolder in that it does not get loaded and pushed to the client. This is a good place for static assets like images.

#### `server` subfolder

```
server
  main.js
```

This is where all the server magic happens. You'll want to set up your publications here. `main.js` will be loaded before anything else in this folder.

#### `tests` subfolder

```
tests
```

This subfolder is ignored by Meteor. There are incredible packages that exist for testing your apps locally like [velocity:core](https://github.com/meteor-velocity/velocity/). I have included this `tests` folder in my structure as a placeholder. I may add some sample testing in the future.

#### Important

* Paying attention to the directory structure is important to the security of your app. Be mindful of sensitive data and where it's located.
* The first thing to do when troubleshooting an unfamiliar bug is check it's location and ask yourself who's trying to access it.

## Features

#### Inline Edits for Admins

If you log in as an Admin you can change the title and subtitle of the home and about us pages by clicking on it, editing it, and clicking away. This is a reactive source that will update the view for everyone connected. This is a simple proof of concept that I hope to expand for paragraphs and other areas of the site. Once it is done, I will add validation, clean it up and spin it out into a meteor package.

#### Chatroom

This area looks like a separate SPA though it's part of the entire app. The majority of the work for the chat app is located in `client/templates/chat`. The functionality is slowly coming along. Keep checking back here for updates.

## Resources

* [Full Meteor Documentation](http://docs.meteor.com/#/full/)
* [AtmoshpereJS](https://atmospherejs.com/) - Search Meteor Packages
* [Discover Meteor Book](https://www.discovermeteor.com/)

###### Notice

I am not affiliated with Meteor or any of the packages used above. If you have issues or questions regarding Meteor or those packages, I'd do my best to help, but they would be better directed to their respective owners. Any and all comments and suggestions on improving this particular boilerplate are welcomed and encouraged. Enjoy! :)
