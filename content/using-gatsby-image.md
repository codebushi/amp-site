---
title: An Introduction To Using Gatsby Image & Gatsby.js V2
date: "2018-09-20"
path: "/using-gatsby-image/"
image: "img/gatsby-image-v2.jpg"
description: "Updated to Gatsby.js V2! Gatsby-image is a React component that works with Gatsby.js to give you an easy way to load and optimize images on a website."
featured: true
imagewidth: 920
imageheight: 503
---

*This post has been updated for Gatsby.js V2. If you are on V1, here is an [older article](/using-gatsby-image-version1/) for legacy users.*

<a href="https://www.gatsbyjs.org/blog/2018-09-17-gatsby-v2/" target="blank">Gatsby.js V2</a> was recently launched and there have been a few small changes to how <a href="https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-image" target="blank">Gatsby Image</a> is implemented. Gatsby Image is a React component that makes it easy to optimize all the images on your website. It will resize images for you, so you don't load huge images on a mobile device, and it will also lazy load your images with a cool "blur-up" effect so that your initial page loads are blazing fast. If you're new to Gatsby, I highly recommend going through their <a href="https://www.gatsbyjs.org/tutorial/" target="blank">official tutorial</a> first and familiarize yourself with how Gatsby works.

Adding Gatsby Image to your static website can be a bit tricky, especially since Gatsby uses GraphQL to query and load your images before they can be used. Here's a breakdown of the steps needed:

1) Install the required npm packages and configure your `gatsby-config.js` settings.

2) Test that you can query for your images using GraphQL.

3) Choose which image type you will need, fixed or fluid, and add the query to your page.

4) Use Gatsby Image `<Img>` tags on your page.

Here's a demo of the final product:

<h4 class="mt-4 mb-4"><a href="https://gatsby-image-v2.surge.sh/">Gatsby Image Demo</a> <small>( <a href="https://github.com/codebushi/gatsby-image-v2">view source</a> )</small></h4>

<h3 class="mt-5 mb-3">Installing & Configuring Gatsby Image</h3>

We'll start off by installing the <a href="https://github.com/gatsbyjs/gatsby-starter-default" target="blank">default Gatsby Starter</a>. You can clone the repo or use the Gatsby CLI to install the starter.

```bash
gatsby new image-demo https://github.com/gatsbyjs/gatsby-starter-default
cd image-demo/
```

If you used the CLI, you'll need to continue with `yarn` since the initial packages were installed with `yarn` and there will be a yarn.lock file. If you cloned the repo and used `npm install`, then continue to use `npm` so you don't mix the package installers. I'll be using `yarn` for the rest of this demo.

Install Gatsby Image
```bash
yarn add gatsby-image
```

We'll also need three other packages, [gatsby-transformer-sharp](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-transformer-sharp), [gatsby-plugin-sharp](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-sharp), and [gatsby-source-filesystem](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-source-filesystem). If you are not using the default starter and already have these packages installed, you can skip this step.

```bash
yarn add gatsby-transformer-sharp gatsby-plugin-sharp gatsby-source-filesystem
```

The `gatsby-source-filesystem` package allows Gatsby to use GraphQL on the images in a certain directory and make queries out of them. The two `sharp` plugins are what processes the images before you display them.

Open up your `gatsby-config.js` and add the plugins to it. I'll add them right before the existing plugins. Your file should look like this:

```javascript
module.exports = {
  siteMetadata: {
    title: 'Gatsby Default Starter',
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        path: `${__dirname}/src/images`,
        name: 'images',
      },
    },
    'gatsby-transformer-sharp',
    'gatsby-plugin-sharp',
    'gatsby-plugin-react-helmet',
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: 'gatsby-starter-default',
        short_name: 'starter',
        start_url: '/',
        background_color: '#663399',
        theme_color: '#663399',
        display: 'minimal-ui',
        icon: 'src/images/gatsby-icon.png', // This path is relative to the root of the site.
      },
    },
    'gatsby-plugin-offline',
  ],
}

```
**Important:** Make sure you specify the correct `path` to your images! The `gatsby-source-filesystem` will look in this folder to access your images. Since we're using the default starter, there's already a folder at `/src/images` so we'll use that. Get some images off of <a href="https://unsplash.com/" target="blank">Unsplash</a> and add them to that folder.

<h3 class="mt-5 mb-3">Testing An Image Query With GraphQL</h3>

With the plugins installed, we can fire up our site in dev mode.

```bash
gatsby develop
```

Navigate to `http://localhost:8000/` to see your site in dev mode. Now we'll play with the GraphiQL interface to understand how the image query works. Head to `http://localhost:8000/___graphql` to see the GraphiQL view of the site. Here we can test the different queries available to us. I've added 3 images to my `/src/images` folder and named them `one.jpg` `two.jpg` and `three.jpg`. To query for `one.jpg` I'll use this:

```graphql
query {
  imageOne: file(relativePath: {eq: "one.jpg"}) {
    childImageSharp {
      fluid(maxWidth: 1000) {
        base64
        tracedSVG
        aspectRatio
        src
        srcSet
        srcWebp
        srcSetWebp
        sizes
        originalImg
        originalName
      }
    }
  }
}
```

If you hit the play button, you should see data in the response column. This proves that Gatsby is able to find your image and process it.

<amp-img src="../img/gatsby-image-query.png" alt="Gatsby Image Query" layout="responsive" width="1160" height="467"></amp-img>

Try changing `file(relativePath: {eq: "one.jpg"})` to the other images in that folder, and make sure you see the data return.

<h3 class="mt-5 mb-3">Adding The GraphQL Query</h3>

We can now copy this query and use it in our homepage component. Open up `src/pages/index.js`. You'll need to import `graphql` from `'gatsby'` as well as `Img` from `'gatsby-image'`. We'll add the query to the page, the final result looks like this:

```jsx
import React from 'react'
import { Link, graphql } from 'gatsby'
import Img from 'gatsby-image'

import Layout from '../components/layout'

const IndexPage = (props) => (
  <Layout>
    <h1>Hi people</h1>
    <p>Welcome to your new Gatsby site.</p>
    <p>Now go build something great.</p>
    <Link to="/page-2/">Go to page 2</Link>
  </Layout>
)

export default IndexPage

export const pageQuery = graphql`
  query {
    imageOne: file(relativePath: { eq: "one.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 1000) {
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`
```

The query looks a bit different than before, we've removed all the fields inside `fluid(maxWidth: 1000) {}` and used `...GatsbyImageSharpFluid`, which is a "query fragment". Due to some limitations, we were not able to play with `...GatsbyImageSharpFluid` before in GraphiQL, but we can add it here. You can read more about the different fragments on the <a href="https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-image" target="">Gatsby Image Readme</a>.

**Important:** Notice how the `file(relativePath: { eq: "one.jpg" })` part remains the same, this is because the `relativePath` is not relative to `index.js` but rather the folder you specified earlier in `gatsby-config.js` and the `gatsby-source-filesystem`. There is no need to change anything about the `relativePath`.

Gatsby Image has two types of responsive images, `fixed` and `fluid`. This distinction will vary what your query looks like. A `fixed` query has a set width and height and is for supporting different *screen resolutions*. A `fluid` query has a max-width and sometimes a max-height, and will create multiple images for supporting different *screen sizes*. For the most part, I find myself using the `fluid` type since my images will vary depending on the size of the screen. If you want to use the `fixed` type or wish to learn more about the two, check out the <a href="https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-image" target="">Readme</a>.

<h3 class="mt-5 mb-3">Using The Gatsby Image Component</h3>

So we have our query on the page, the GraphQL data can be accessed via `props` in our `IndexPage` component. The full path to the data is `props.data.imageOne.childImageSharp.fluid`. We can pass this into the `<Img>` component like so:

```jsx
<Img fluid={props.data.imageOne.childImageSharp.fluid} />
```

You can destructure this however you like, I'm using the full path for clarity. The image should now be displaying on your dev site! To get all three images, just copy and paste the `imageOne` blocks and rename to `imageTwo` and `imageThree`. You can call these whatever you want, just make sure it matches whatever you pass into the `<Img />` component.

```graphql
query {
  imageOne: file(relativePath: { eq: "one.jpg" }) {
    childImageSharp {
      fluid(maxWidth: 1000) {
        ...GatsbyImageSharpFluid
      }
    }
  }
  imageTwo: file(relativePath: { eq: "two.jpg" }) {
    childImageSharp {
      fluid(maxWidth: 1000) {
        ...GatsbyImageSharpFluid
      }
    }
  }
  imageThree: file(relativePath: { eq: "three.jpg" }) {
    childImageSharp {
      fluid(maxWidth: 1000) {
        ...GatsbyImageSharpFluid
      }
    }
  }
}
```

The components would look like this:

```jsx
<Img fluid={props.data.imageOne.childImageSharp.fluid} />
<Img fluid={props.data.imageTwo.childImageSharp.fluid} />
<Img fluid={props.data.imageThree.childImageSharp.fluid} />
```

We're repeating a lot of the same stuff inside that query, it can be cleaned up by making a custom fragment. Pull out the `childImageSharp` blocks and make a new fragment like so:

```jsx
export const fluidImage = graphql`
fragment fluidImage on File {
  childImageSharp {
    fluid(maxWidth: 1000) {
      ...GatsbyImageSharpFluid
    }
  }
}
`;
```

We can then replace the repeating code with this new fragment like so:

```jsx
export const pageQuery = graphql`
  query {
    imageOne: file(relativePath: { eq: "one.jpg" }) {
      ...fluidImage
    }
    imageTwo: file(relativePath: { eq: "two.jpg" }) {
      ...fluidImage
    }
    imageThree: file(relativePath: { eq: "three.jpg" }) {
      ...fluidImage
    }
  }
`
```

We'll now have all three images on our homepage! You can play around with the different Gatsby fragments for different loading effects. `...GatsbyImageSharpFluid` will give the "blur up" effect, try `...GatsbyImageSharpFluid_tracedSVG` for a different effect and experiment with fixed images.

<h4 class="mt-4 mb-4"><a href="https://gatsby-image-v2.surge.sh/">Gatsby Image Demo</a> <small>( <a href="https://github.com/codebushi/gatsby-image-v2">view source</a> )</small></h4>