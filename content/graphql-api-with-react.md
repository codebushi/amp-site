---
title: GraphQL API with React.js
date: "2018-04-17"
path: "/graphql-api-with-react/"
image: "img/graphql-with-react.jpg"
description: "A tutorial for using modern GraphQL APIs with React.js. We'll use the GraphiQL explorer to construct our query and then use React and axios to make the API request."
featured: true
tags: ["blog"]
imagewidth: 920
imageheight: 503
---

Many new APIs are moving away from REST and embracing the change to [GraphQL](http://graphql.org/). GitHub's [API V4](https://developer.github.com/v4/) is one example of this, and that's because GraphQL offers a lot more flexibility when querying for data. This tutorial will go over how to incorporate and make queries using React and a modern GraphQL API.

For this demo, we'll be using [AniLists's V2 API](https://github.com/AniList/ApiV2-GraphQL-Docs) to get a list of anime titles and their cover images. I chose this API because it's ready to use without any configurations or OAuth tokens. GitHub's V4 API works in a similar way, but you'll need to create an access token and include it with each request. I'll also be using [axios](https://github.com/axios/axios) to make our queries, this should give a general overview on the basics of using GraphQL and React.

<h4 class="mt-4 mb-4"><a href="http://graphql-react.surge.sh/">GraphQL API with React Demo</a> <small>( <a href="https://github.com/codebushi/graphql-anime">view source</a> )</small></h4>

To keep things simple, I'm going to use [create-react-app](https://github.com/facebook/create-react-app) to start things off. Once you have create-react-app installed, we can create our app.

```bash
# Create the app
npx create-react-app anime-graphql

cd anime-graphql

# Install axios, use 'npm install axios' if you don't use yarn
yarn add axios

# Start the app, browse to http://localhost:3000/
yarn start
```

We now have our generic app running and `axios` is the only additional package we needed to install. Open up `src/App.js` and strip out the boilerplate JSX until we're left with this

```jsx
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div>

      </div>
    );
  }
}
export default App;
```

<h3 class="mt-5 mb-3">GraphiQL Explorer</h3>

AniList has some pretty good [documentation](https://anilist.gitbooks.io/anilist-apiv2-docs/graphql/getting-started.html) for their API, including an example GraphQL `query`. Their example is to get a single anime title by providing an ID. If we want to get a big list of cover images, we'll need to check out their GraphiQL explorer. In order to use this feature, you'll need to [register](https://anilist.co/register) for free with AniList. You only need to register if you want to play with their explorer, it's **not required** for our React app.

Most GraphQL APIs will come with an explorer where you can make test queries and search for possible values. Let's check out AniList's https://anilist.co/graphiql.

<amp-img src="../img/react-graphiql.png" alt="GraphiQL Preview" layout="responsive" width="1383" height="741"></amp-img>

The left pane is where you can make a query and the right pane is the result once you hit the "play" button. The far right has a Documentation Explorer which shows all the different fields and values. The left pane also has an auto-complete feature, if you hit `control+space` within a bracket, it will show a popup of all available fields. This is one of the best features of a GraphQL API since you can quickly and easily make a query and just copy and paste it into your app.

We can plug in a test query to see this in action. Copy the following into the left pane and hit play

```javascript
{
  Media(id: 1) {
    id
    title {
      romaji
      english
      native
    }
  }
}
```

This will make a `Media` query with `id: 1`, and get all the titles associated with it. If you put your cursor above `title` and hit `control+space`, you can select any other field from the auto-complete and query for that as well. A GraphQL query will only return the fields you ask for, which eliminates the over-fetching of unnecessary data.

To get a list of titles, we'll need to query for a `Page` and then add `media()` within that query. Search for `Page` in the Document Explorer and it will list some additional information. Our query will look something like this:

```javascript
{
  Page {
    media(isAdult: false, sort: POPULARITY_DESC) {
      id
      title {
        romaji
        english
      }
      coverImage {
        large
      }
    }
  }
}
```

Note that `media()` requires at least one argument to work, so we'll set isAdult to false and add a sort. We can pass additional filters into the media query, like the `season` and `seasonYear`. Play around with the auto-complete and the documentations to find available fields. We're also asking for the coverImage, which will be a url to the image.

<h3 class="mt-5 mb-3">Making the Query with React.js</h3>

Once we are happy with the query, we can set things up in our React app. Be sure to import `axios` and set up some initial state. Using async/await, we create a function called `getAnime` to request our data. An [async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) is newer syntax for handling Promises in JavaScript.

```jsx
// Be sure to import axios
import axios from 'axios';

class App extends Component {

state = {
  error: null,
  isLoaded: false,
  items: []
}

getAnime = async (query, variables) => {
  try {
    const response = await axios.post('https://graphql.anilist.co', {
      query,
      variables
    });

    // Log the response so we can look at it in the console
    console.log(response.data)

    // Set the data to the state
    this.setState(() => ({
      isLoaded: true,
      items: response.data.data.Page.media
    }));

  } catch (error) {
    // If there's an error, set the error to the state
    this.setState(() => ({ error }))
  }
}
```

The one and only AniList endpoint is `https://graphql.anilist.co` which we have to `POST` to. Using `axios.post()`, we pass in the endpoint as the first argument and an object with the query as the second argument. When the query is complete, we set the data to the `response` constant and update the state. Let's add a `componentDidMount()` lifecycle method and call `this.getAnime()` with our `query`.

```jsx
componentDidMount() {

  // This is the GraphQL query
  const query = `
  query {
    Page {
      media(isAdult: false, sort: POPULARITY_DESC) {
        id
        title {
          romaji
          english
        }
        coverImage {
          large
        }
      }
    }
  }
  `;

  // These variables are optional, leave empty for now
  const variables = {};

  // We call the method here to execute our async function
  this.getAnime(query, variables)

}
```

For the `query` constant, we can basically copy out what we have from the GraphiQL explorer and paste it into a template string, adding `query` before the first bracket. The `variables` constant is optional and it's a way of passing variables into the query. Finally we call `this.getAnime()` and pass in the variables. Open up your browser's inspector and head into the Network tab, you should see the request and the response in the console!

<h3 class="mt-5 mb-3">Rendering the Data</h3>

With the data in hand, the hard part is over. Only thing left is to update our `render()` method and map out our cover images. We'll also create a simple component to house each item.

```jsx
// Our simple component for each grid item
const GridItem = (props) => (
  <div className="grid__flex">
    <img className="grid__img" src={props.image} />
  </div>
)

class App extends Component {
...

// The new render()
render() {

  const { error, isLoaded, items } = this.state;

  if (error) {
    return <div>{error.message}</div>;
  } else if (!isLoaded) {
    return <div>Loading...</div>;
  } else {

    return (
      <div className="grid">
        {items.map(item => (
          <GridItem key={item.id} image={item.coverImage.large} />
        ))}
      </div>
    );

  }
}
```

You should be seeing the images in your browser! Let's spice things up a bit with some CSS Grid, open up `src/App.css` and replace with the following.

```css
html, body {
  margin: 10px;
}

.grid {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  grid-auto-flow: dense;
}

.grid__flex {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.grid__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

Nice, the images should now be showing in a responsive grid! Now try creating a new query with the explorer and copy/paste it into the React App. You can also try out [GitHub's GraphQL API](https://developer.github.com/v4/) and play with their explorer as well, everything works pretty much the same except you'll need to generate an OAuth token and add it to your request headers. Here's a sample of what a GitHub query function looks like.

```jsx
getRepo = async (query, variables) => {
  try {
    const response = await axios.post(
      "https://api.github.com/graphql",
      {
        query: gitQuery,
        variables: variables
      },
      {
        headers: {
          Authorization: "token YOUR_TOKEN_HERE"
        }
      }
    );
    console.log(response.data);
  } catch (error) {
    console.log(error);
  }
};
```

<h4 class="mt-4 mb-4"><a href="http://graphql-react.surge.sh/">GraphQL API with React Tutorial</a> <small>( <a href="https://github.com/codebushi/graphql-anime">view source</a> )</small></h4>