## A Simpler React & Rails Integration

I was recently tasked with adding React to a fully-functioning production Rails app. Naturally I looked at a couple of gems out there, including [react_on_rails](https://github.com/shakacode/react_on_rails), the kitchen sink of Rails-React gems, and [react-rails](https://github.com/reactjs/react-rails), which takes a more pared-down approach, and tried both of them. I appreciated the attitude taken by [@shakacode](https://twitter.com/shakacode), maintainer of react_on_rails, in terms of making JS and React first-class citizens, as I strongly believe that using proper client-side tools makes JS development a joy rather than a pain.

Unfortunately I was still experiencing pain with the integration due to some aspects of our code base. Instead, I opted to do things manually and aim for a simpler integration. I reached out on Twitter to see how other people were integrating the two and received great ideas from some outstanding community members, including [Javier Cuevas](https://twitter.com/javier_dev), [Ricardo Vega](https://twitter.com/bigardone), [Jack Callister](https://twitter.com/jarsbe), and others).

My requirements were simple: I wanted to use ES6, and I wanted to use modules the way I was accustomed to for React and React Native development. I haven't added Redux yet, but it is certainly possible with this appraoch.

### Add Webpack

There are a few issues you need to consider when adding React. The first is adding your modern ES6 module-based React code to your Rails JS bundle. The way to do this is by having **webpack** process your React code and include the generated JS file (or files) into the asset pipeline.

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
