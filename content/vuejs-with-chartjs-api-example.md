---
title: Vue.js and Chart.js Weather API Example
date: "2018-09-13"
path: "/vuejs-with-chartjs-api-example/"
image: "img/vuejs-chartjs.jpg"
description: "Use Vue.js and Chart.js to add charts/graphs to an existing website. Get weather data using Axios and the OpenWeatherMap API, then chart the temperatues by city."
featured: true
imagewidth: 920
imageheight: 503
---

Recently I was asked to add some fancy charts and graphs to an existing Wordpress website. In the past, my weapon of choice would be either jQuery or Angular.js for a task such as this, but I decided to try the popular [Vue.js](https://vuejs.org/). Vue.js is a progressive javascript framework, which means it's extremely easy to add it to an existing website and get started. For this tutorial, I'm going to start with a simple HTML boilerplate file and add Vue.js at the bottom in a `<script>` tag. We'll hit the OpenWeatherMap API to get some weather data and chart the temperatures with Chart.js.

If you're new to Vue.js, I highly recommend going through their [introduction guide](https://vuejs.org/v2/guide/) first. It covers some important basics and the structure of a Vue application.

<h4 class="mt-4 mb-4"><a href="http://vuejs-chartjs.surge.sh/">Vue.js and Chart.js Weather API Example</a> <small>( <a href="https://github.com/codebushi/vuejs-chartjs-example">view source</a> )</small></h4>

Before we start, I want to note that Vue.js also has a [CLI](https://cli.vuejs.org/), which works similar to React's Create-React-App. The Vue CLI is great for JavaScript apps built with Vue.js from the beginning. For our purposes, however, we'll be adding Vue to an "existing site" and won't need to use the CLI. This also means we don't need to worry about any Webpack configurations or build/deploy commands.

Our existing HTML file looks like this:

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <title>Vue.js Example With Chart.js</title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.2/css/bootstrap.min.css" integrity="sha384-Smlep5jCw/wG7hdkwQ/Z5nLIefveQRIY9nfy6xoR1uRYBtpZgI6339F5dgvm/e9B" crossorigin="anonymous">
  </head>
  <body>
    <div id="app">
      <div class="container">
        <div class="my-5">
          <form>
            <div class="row">
              <div class="col-md-6 offset-md-3">
                <h5>Enter A City:</h5>
                <div class="input-group">
                  <input type="text" class="form-control">
                  <div class="input-group-append">
                    <button class="btn btn-outline-secondary" type="submit">Submit</button>
                  </div>
                </div>
              </div>
            </div>
          </form>
        </div>
        <div class="my-5">
          <div class="alert alert-info">
            Loading...
          </div>
          <div>
            <canvas id="myChart"></canvas>
          </div>
        </div>
      </div>
    </div>
  </body>
</html>
```

It's a simple HTML file with some Bootstrap grid and form elements. Imagine this being any existing website you'll need to work on.

<h3 class="mt-5 mb-3">Adding Vue.js and Chart.js</h3>

In order to use Vue.js and the other JavaScript tools, we'll need to add their `<script>` tags to our page. If you have access to the site's entire template, these can be added right after the `</body>` tag. If you don't want to load these scripts on every page of your site, they can be added to the bottom of whichever page you are working on.

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.13.0/moment.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.18.0/axios.min.js"></script>
<script src="main.js"></script>
```

Here we load Vue.js, Moment.js for date/time formatting, Chart.js, Axios to make our API requests, and main.js which will contain our own code. Note that these scripts are loaded one by one, which isn't ideal. One of the benefits of using the Vue CLI is the ability to bundle all the necessary JavaScript files and optimize loading, but we don't always have that luxury when working with old websites.

<h3 class="mt-5 mb-3">Using Axios to get weather data from the API</h3>

The first step of this app is getting some weather data from the OpenWeatherMap API. I chose to use the [5 day forecast](https://openweathermap.org/forecast5) since it's available on the free tier. You can sign up for free with OpenWeatherMap to get your own appid key, which is needed at the end of every query.

In our `main.js` we'll initialize our Vue app and create a method to get our data.

```javascript
var app = new Vue({
  el: "#app",
  data: {
    chart: null,
    city: "",
    dates: [],
    temps: [],
    loading: false,
    errored: false
  },
  methods: {
    getData: function() {

      this.loading = true;

      axios
        .get("https://api.openweathermap.org/data/2.5/forecast", {
          params: {
            q: this.city,
            units: "imperial",
            appid: "Your OpenWeatherMap Key here"
          }
        })
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    }
  }
});
```

The `getData` method contains the Axios query we'll be making. The only missing field we need is `this.city` which we'll need from the form. Let's hook up our HTML to this Vue app.

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <title>Vue.js Example With Chart.js</title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.2/css/bootstrap.min.css" integrity="sha384-Smlep5jCw/wG7hdkwQ/Z5nLIefveQRIY9nfy6xoR1uRYBtpZgI6339F5dgvm/e9B" crossorigin="anonymous">
  </head>
  <body>
    <div id="app">
      <div class="container">
        <div class="my-5">
          <form v-on:submit.prevent="getData">
            <div class="row">
              <div class="col-md-6 offset-md-3">
                <h5>Enter A City:</h5>
                <div class="input-group">
                  <input type="text" class="form-control" v-model="city">
                  <div class="input-group-append">
                    <button class="btn btn-outline-secondary" type="submit">Submit</button>
                  </div>
                </div>
              </div>
            </div>
          </form>
        </div>
        <div class="my-5">
          <div class="alert alert-info" v-show="loading">
            Loading...
          </div>
          <div v-show="chart != null">
            <canvas id="myChart"></canvas>
          </div>
        </div>
      </div>
    </div>
  </body>
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.13.0/moment.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.18.0/axios.min.js"></script>
  <script src="main.js"></script>
</html>
```

As you can see, most of the HTML is unchanged. The only parts that need to be added are the various `v-` attributes. Let's quickly go over the changes.

```html
<form v-on:submit.prevent="getData">
```

This line hooks up our form to our `getData` method, and fires whenever the form is submitted. The default action of a form submit is to reload the page, so the `.prevent` acts like a `event.preventDefault()`.

```html
<input type="text" class="form-control" v-model="city">
```

This binds the input field to our Vue app's `data.city`, and can then be access in the `getData` method via `this.city`. This is how we change the city based on the user input.

```html
<div class="alert alert-info" v-show="loading">
Loading...
</div>
```

The `v-show` parts are optional, but it makes for a better UI experience. It will show the loading alert when the app is in the loading state, and hide it once the loading is complete, after the query successfully gets the data.

With these pieces in place, you can try our the query. Enter a city and submit the form. Open up your console and you should see the log of data from the API!

<h3 class="mt-5 mb-3">Formatting the data for Chart.js</h3>

Once we get our data, we'll need to organize it into arrays for Chart.js to use. We will have an array of dates for the x-axis and an array of temperature values for the y-axis.

In our `main.js`, replace the `console.log(response)` with the following:

```javascript
.then(response => {
  this.dates = response.data.list.map(list => {
    return list.dt_txt;
  });

  this.temps = response.data.list.map(list => {
    return list.main.temp;
  });
})
```

This sets our `data.dates` and `data.temps` to the mapped arrays. Note that we are using an arrow function in the `.then()`, which is binding the `this.dates` and `this.temps` properly. If you can't use arrow functions, be sure to add `.bind(this)` after your function.

<h3 class="mt-5 mb-3">Charting the data with Chart.js</h3>

Once the data is formatted, we can initialize a new Chart and plug in the data. When working with Chart.js, or any other charting library, much of the legwork is formatting the chart properly. I highly recommend going through the [Chart.js docs](http://www.chartjs.org/docs/latest/) and checking out their samples to see the different options available. Depending on the data you are charting, the tooltips and X or Y axis may need to be adjusted to display the proper units for the data.

I've gone through and formatted the Chart.js options already for this demo, feel free to look at the options and make any changes needed.

Our final `main.js` file now looks like this:

```javascript
var app = new Vue({
  el: "#app",
  data: {
    chart: null,
    city: "",
    dates: [],
    temps: [],
    loading: false,
    errored: false
  },
  methods: {
    getData: function() {
      this.loading = true;

      if (this.chart != null) {
        this.chart.destroy();
      }

      axios
        .get("https://api.openweathermap.org/data/2.5/forecast", {
          params: {
            q: this.city,
            units: "imperial",
            appid: "fd3150a661c1ddc90d3aefdec0400de4"
          }
        })
        .then(response => {
          this.dates = response.data.list.map(list => {
            return list.dt_txt;
          });

          this.temps = response.data.list.map(list => {
            return list.main.temp;
          });

          var ctx = document.getElementById("myChart");
          this.chart = new Chart(ctx, {
            type: "line",
            data: {
              labels: this.dates,
              datasets: [
                {
                  label: "Avg. Temp",
                  backgroundColor: "rgba(54, 162, 235, 0.5)",
                  borderColor: "rgb(54, 162, 235)",
                  fill: false,
                  data: this.temps
                }
              ]
            },
            options: {
              title: {
                display: true,
                text: "Temperature with Chart.js"
              },
              tooltips: {
                callbacks: {
                  label: function(tooltipItem, data) {
                    var label =
                      data.datasets[tooltipItem.datasetIndex].label || "";

                    if (label) {
                      label += ": ";
                    }

                    label += Math.floor(tooltipItem.yLabel);
                    return label + "°F";
                  }
                }
              },
              scales: {
                xAxes: [
                  {
                    type: "time",
                    time: {
                      unit: "hour",
                      displayFormats: {
                        hour: "M/DD @ hA"
                      },
                      tooltipFormat: "MMM. DD @ hA"
                    },
                    scaleLabel: {
                      display: true,
                      labelString: "Date/Time"
                    }
                  }
                ],
                yAxes: [
                  {
                    scaleLabel: {
                      display: true,
                      labelString: "Temperature (°F)"
                    },
                    ticks: {
                      callback: function(value, index, values) {
                        return value + "°F";
                      }
                    }
                  }
                ]
              }
            }
          });
        })
        .catch(error => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    }
  }
});
```

Most of the chart is made up of options, which never really change after they are set to your liking. The parts that do change are `data.labels` and `data.datasets`. You can see that we plug in our `this.dates` and `this.temps` arrays into these areas of the chart.

We also set `this.chart` to the newly created chart. Notice this new condition right before the Axios query:

```javascript
if (this.chart != null) {
  this.chart.destroy();
}
```

This is used when more than once city is queried, it will destroy the old chart before making a new one to avoid any data overlapping.

That's it! The app now gets data based on the city you input and charts out the temperatures. Try experimenting with the different Chart.js options or adding a toggle for a °F to °C.

<h4 class="mt-4 mb-4"><a href="http://vuejs-chartjs.surge.sh/">Vue.js and Chart.js Weather API Example</a> <small>( <a href="https://github.com/codebushi/vuejs-chartjs-example">view source</a> )</small></h4>