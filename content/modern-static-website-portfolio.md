---
title: Creating a Modern Static Website Portfolio
date: "2018-02-24"
path: "/modern-static-website-portfolio/"
image: "./img/static-website-portfolio.jpg"
description: "A digital portfolio is vital for any web developer. We'll explore some modern static site generators and free hosting solutions to get your personal site up and running."
tags: ["blog"]
---

If you're a web developer or someone who's looking to get into the field, having a digital portfolio can be extremely valuable. A personal website is a fantastic way to showcase your work and help you get noticed. Fortunately, getting a website up and running is easier and cheaper than ever. With the rise of static site generators and hosting solutions, creating an online portfolio can be relatively easy and completely free! Let's explore some resources and the steps needed to get started.

<h2 class="mt-5 mb-3">Static Site Generators</h2>

Static site generators are all the rage these days and it's easy to see all the benefits they offer. The most popular static site generators are open source and free to use, and the sites they produce are much simpler than dynamic CMS systems. Since a static website doesn't rely on a database or server side processes, it can be hosted for free by a variety of providers and generally has much better performance.

[StaticGen](https://www.staticgen.com/) is a great resource for finding the most popular generators. Although there are many to choose from, they all basically work the same way.

<ul class="u-list-spaced">
    <li><em>Choose a static site generator and install it.</em> This process will vary depending on the language it's written in. Refer to the documentation for installation instructions.</li>
    <li><em>Choose a theme or create your own.</em> Much like Wordpress, most static site generators have themes already created and offered for free. Generators that are more popular will usually have more themes to choose from. If you're a more experienced developer, then creating your own theme is always an option.</li>
    <li><em>Add your content to the site.</em> You'll have to refer to the site generator's documentation, but adding your content should be pretty straightforward based on the theme you select.</li>
    <li><em>Run the build process.</em> The static site generator will take all of your content and compile it, returning a folder that contains your entire website. You can host this folder in a wide variety of places, many of which are free.</li>
</ul>

<h3 class="mt-5 mb-3">Jekyll</h3>

As the most popular static site generator, [Jekyll](https://jekyllrb.com/) has been battle tested and is the engine behind Github Pages. There is a large community of support and a huge library of free themes available at [jekyllthemes.org](http://jekyllthemes.org/). It's written in Ruby, so you'll need to first [install Ruby](https://www.ruby-lang.org/en/downloads/) and [Ruby Gems](https://rubygems.org/pages/download).

```bash
# Install Jekyll and Bundler gems through RubyGems
gem install jekyll bundler

# Create a new Jekyll site at ./mysite
jekyll new mysite

# Change into your new directory
cd mysite

# Build the site on the preview server
bundle exec jekyll serve

# Now browse to http://localhost:4000
```

Check out their [quick start](https://jekyllrb.com/docs/quickstart/) guide for more site options and [theme instructions](https://jekyllrb.com/docs/themes/).

<h3 class="mt-5 mb-3">Hugo</h3>

[Hugo](https://gohugo.io/) is written in the [Go](https://golang.org/) programming language and is extremely fast when it comes to compiling a site. It has seen a tremendous surge in popularity and also has a great selection of [themes](https://themes.gohugo.io/). To check it out on `MacOS` you'll need to first install [Homebrew](https://brew.sh/), which will help you install Hugo.

```bash
# Install Hugo using Homebrew
brew install hugo

# Verify that Hugo is installed
hugo version

# Create a new site called 'quickstart' using Hugo
hugo new site quickstart
cd quickstart
```

Hugo doesn't come with a default theme, so you'll get a blank site if you try to run the development server. Let's add a theme so we have something to look at.

```bash
# To copy a theme from github, we'll need to init a new repo and add a submodule
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke

# Add the new theme to Hugo's configuration file
echo 'theme = "ananke"' >> config.toml

# Adding some content
hugo new posts/my-first-post.md

# Browse to http://localhost:1313/ to see your Hugo dev server!
hugo server -D
```

Pretty simple, and blazing fast build times! To learn more about Hugo, visit their [getting started](https://gohugo.io/getting-started/) guide.

<h3 class="mt-5 mb-3">Gatsby.js <small>(Personal Favorite)</small></h3>

Although [Gatsby.js](https://www.gatsbyjs.org/) is relatively new to the world of static site generators, it brings a lot of modern web technologies to the table. It's built with Javascript and features tools such as React.js, Webpack, and and GraphQL. It's my personal favorite because it allows me to build using React components and Javascript, which is the language I'm most familiar with. Instead of traditional themes, Gatsby users can select from a list of [starters](https://www.gatsbyjs.org/docs/gatsby-starters/) which are partially built websites.

There are not as many Gatsby.js starters as Jekyll or Hugo themes, but hopefully this will change in the future. I've started my own collection of Gatsby starters as a way of contributing to the community, check out the free [Code Bushi Gatsby starters](https://codebushi.com/gatsby-starters/).

To install Gatsby, you'll first need to have [Node.js](https://nodejs.org/en/download/) and npm installed on your machine.

```bash
# Install Gatsby's command line tool globally
npm install --global gatsby-cli

# Creates a new Gatsby site, using the Dimension starter
gatsby new gatsby-site https://github.com/ChangoMan/gatsby-starter-dimension

# Move into your new directory
cd gatsby-site

# Start up the Gatsby server, browse to http://localhost:8000/
gatsby develop
```

If you're familiar with React.js, then a Gatsby site will feel right at home. All the pages are composed of React components, and you can easily install new components via npm. Go through their [official tutorial](https://www.gatsbyjs.org/tutorial/) to learn more about Gatsby's powerful GraphQL data structure.

Choosing the right static site generator is all about personal preference and tinkering with the different options. Hopefully this article was able to help narrow down some of the most popular choices available today. Once your site is built and compiled, it's time for hosting. In <a href="https://codebushi.com/hosting-your-static-website/">the next post</a>, we'll explore some popular hosting options which include GitHub Pages, Surge.sh, and Netlify.
