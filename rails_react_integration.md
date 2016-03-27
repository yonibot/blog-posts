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

We've added a single loader - Babel - to process our ES6 as well as our React inline Javascript syntax (jsx), and we tell it to exclude `node_modules` as that would just be a waste of time. The `resolve.extensions` section tells Webpack which file types should be processed, and the rest is simply telling Webpack the location of an entry Javascript file and where to spit out the bundle. However, our Babel loader and plugins must be installed via npm:

* ```npm install --save babel-loader babel-core babel-preset-react babel-preset-stage-0 babel-preset-es2015```

Test this out now by typing `webpack` in the terminal at the root of your app, and your JS bundle should be produced at the filepath that you provided.

## Add the JS bundle to your build path

In your `application.js` file, add the following require at the top:

```
//= require app_bundle
```
If you have used `require_tree .` and relinquished control over file loading order, this could be a problem. You may need to remove that file and require each JS file by name and in the proper order.


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
