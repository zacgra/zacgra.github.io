---
title: "Setting up Rails 7 with React from Scratch"
author: zacgra
layout: post
categories: Code
tags: [Rails, React]
---

## Step #1: Install Dependencies

### Install up-to-date versions of Node and Yarn

You can install via Homebrew(MacOS) or ASDF(Mac/Linux/WSL) or any other package/version manager of your choice.

### Install a newer version of Ruby

```
# as an example
asdf install ruby 3.1.3
```

## Install the rails gem

Install the rails gem. If you don't specify the Rails version, the version will be speficied by your Ruby version.

```
gem install rails
```

## Step 2: Install and Configure Rails

### Install Rails 7

This step has changed a bit from Rails 6, so the key is specifying `esbuild` for JavaScript bundling. This will also install the `jsbundling-rails` gem. Skipping hotwire is not required, but might make things a bit less cluttered or conflicting. The database flag, as well as skipping the default testing setup, are also just for reference.

```
rails new railsappname \
    --skip-hotwire --skip-test \
    -d postgresql \
    --javascript=esbuild \
```

### Setup Database

We will need to setup our Postgres database or else Rails will throw an error when we try to start the server.

```
rails db:create
rails db:migrate
```

Running the db:migrate will initialize our db schema even though it is currently empty.

### Start Rails server

Next, we will want to start our Rails server so we can start checking what is being rendered, as well as start bundling for js and css. This can be done with foreman, which is all set up as an existing script in `bin/dev`.

Run the following from the terminal:

```
bin/dev
```

### Set Up a Rails View

The browser should now be rendering a default Rails view. Let's make a new view/controller for our React content.

First, go in to `config/routes.rb` and add a a new route:

```rb
root "homepage#index"
```

Next, in `app/controllers` add a new file called `homepage_controller.rb` and paste the following:

```rb
class HomepageController < ApplicationController
    def index
        render "pages/homepage"
    end
end
```

Finally, in `views`, create a new folder called pages, then add a new file called `homepage.html.erb`. Inside that, paste the following:

```html
<h1>Rails with React</h1>
<p id="react-root">This paragraph is being renderd by Rails</p>
```

Take note of the id we are using here, as this is where we will hook in our React root later.

Now Rails server should be showing our new homepage view when we refresh the browser.

## Step 3: Set up React

### Install React and ReactDOM

Since we already installed yarn, we can just run the following:

```
yarn install react react-dom
```

### Create a component to ensure rendering is working

Next, let's set up the React component we want to render, just to make sure everything is configured correctly.

In `app/javascript`, make a subfolder called `components`. Inside `app/javascript/components`, create a new file called `homepage.jsx` and placing the following code in that file:

```jsx
import React from "react";
import * as ReactDOM from "react-dom/client";

const Homepage = () => {
  return <p>This paragraph is being rendered by React!</p>;
};
```

Earlier, we created `view/pages/homepage.html.erb`. Now lets create a React root using the paragraph with the `react-root` id in that view. We can update our previous component like this:

```jsx
import React from "react";
import * as ReactDOM from "react-dom/client";

const Homepage = () => {
  return <p>This paragraph is being rendered by React!</p>;
};

const root = ReactDOM.createRoot(document.querySelector("#react-root"));
root.render(<Homepage />);
```

### Point our bundler to our new React code

Finally, we just need to make sure our JS bundler knows to look in `components/homepage` for our React code. We can import it by adding the following to `javascript/application.js`:

```js
import "./components/homepage";
```

## All Done

That's it! You should now see the text "This paragraph is being rendered by React!" when you refresh your browser.
