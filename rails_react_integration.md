## A Simpler React & Rails Integration

I was recently tasked with adding React to a fully-functioning production Rails app. There are a couple of excellent gems out there that already provide great integration functionality, including [react_on_rails](https://github.com/shakacode/react_on_rails) and [react-rails](https://github.com/reactjs/react-rails). But in my case, our existing app had some complex asset requirements and I found it to be a good learning experience doing the integration myself.

Ultimately I believe that I'll reach something similar to the integration offered by the gems. But I will have done it to suit my own needs, and I will have learned a lot in the process.

I want to thank a number of outstanding Rails & React community members who provided me with ideas and inspiration in this endeavor, including [Javier Cuevas](https://twitter.com/javier_dev), [Ricardo Vega](https://twitter.com/bigardone), [Jack Callister](https://twitter.com/jarsbe), and others).

My requirements were simple: I wanted to use ES6 and I wanted to use modules the way I was accustomed to for React and React Native development. Lastly, I wanted client-side development to be done right. I strongly believe that if you want to add any significant front-end development to your app, JS must be treated as a first-class citizen. That means that client-side tooling must be sufficiently capable. I have not added Redux yet to our Rails app, but it is possible with this approach. Perhaps for a future post.

### The approach

The basic thinking is to keep Rails and React as two units cohabiting as independent entities. I keep a separate React folder with my containers and components, and use Webpack to process them into one neat JS file. I then include the bundle in Rails' asset pipeline by requiring it in application.js and *Rails is none the wiser*!

### Add Webpack

The first step is to install npm and add webpack, which will take the React components, process them, and make them into a neat JS bundle.

* ```npm install```

Run this in the root of your app. It will add a package.json file and allow you to manage node dependencies. Now install Webpack:

* ```npm install --save webpack```

Webpack can be extensively configured but getting a basic installation up and running is simple. Let's start with the most basic Webpack configuration we can possibly live with:

```
module.exports = {
  entry: './frontend/main.js',
  output: {
    filename: './app/assets/javascripts/app_bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.js?$/,
        exclude: /(node_modules)/,
        loader: 'babel',
        query: {
          presets: ['react', 'es2015', 'stage-0']
        }
      }
    ]
  },
  resolve: {
    extensions: ['.js', '']
  }
};
```

We've added a single loader - Babel - to process our ES6 as well as our React inline Javascript (jsx), and we tell it to exclude `node_modules` as that would just be a waste of time. The `resolve.extensions` section tells Webpack which file types should be processed (the blank string ('') tells Webpack to require modules as well as files), and the rest tells Webpack the location of an entry Javascript file and where to spit out the bundle.

### Using External Dependencies

To use this configuration, we must install the Babel loader and the plugins. Now that we have a *package.json* file, we are free to use npm to install any external dependencies that we want:

* ```npm install --save babel-loader babel-core babel-preset-react babel-preset-stage-0 babel-preset-es2015```

Test the current configuration by typing `webpack` in the terminal at the root of your app, and your JS bundle should be produced at the filepath that you provided.

The reason that I use the `--save` flag rather than `--save-dev` is that by default, npm does not install devDependencies on production. If you'd like to use devDependencies anyway, you can disable production mode for npm by running `heroku config:set NPM_CONFIG_PRODUCTION=false`.

### Add the JS bundle to your build path

In your `application.js` file, add the following require at the top:

```
//= require app_bundle
```
If you have used `require_tree .` and relinquished control over file loading order, you may encounter problems. If that happens, remove that line and require each JS file by name and in the proper order.

## Add your React component

{ Fill this in! Multiple components per page.}

## Deploying to Heroku

I used the [Heroku Multi Buildpack](https://github.com/ddollar/heroku-buildpack-multi) by David Dollar, which enables you to run multiple Heroku buildpacks on deploy:

* `heroku buildpacks:set https://github.com/ddollar/heroku-buildpack-multi.git`

Then add your specific buildpacks to a `.buildpacks` file:
```
https://github.com/heroku/heroku-buildpack-nodejs
https://github.com/heroku/heroku-buildpack-ruby
```

Using the post-install script option in `package.json`, we will have webpack generate our bundle before the Rails buildpack wakes up:

```
  ...
  ,
  "scripts": {
    "postinstall": "webpack --config webpack.config.js"
  }

```



## Bonuses

At this point, the integration is technically complete. One problem that remains is that we still need to issue the `webpack` command every time we want to reload our React code. Recall that Rails loads all of the code on every refresh in the development environment). We can get the same functionality by using the Webpack-dev-server, which watches for changes to our front-end code and runs `webpack` for us every time a change is perceived.

### Webpack Dev Server

{Add this}

### Bootstrap


* gitignore node_modules
* add ES6
* add webpack-dev-server to prevent pressing 'webpack' all the time
* add heroku multibuildpack  (and set npm config)
* adding bootstrap
* adding npm dependencies (tinymce, etc)
- heroku config:set NPMCONFIGPRODUCTION=false   (or add)
- or movie dependencies out of devDependencies (ideal)
* making react components available (???)
* add jQuery?? (prob not)
