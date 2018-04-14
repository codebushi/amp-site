---
title: Form Handling with Gatsby.js and Netlify
date: "2018-03-25"
path: "/form-handling-gatsby-netlify/"
image: "./img/gatsby-forms.jpg"
description: "Easily handle forms for your Gatsby.js static website with Netlify. Works with most static site generators."
featured: true
tags: ["blog"]
---

Having a working contact form is a basic requirement for many websites, but setting one up with a static site can be tricky. If you're hosting your website with [Netlify](https://www.netlify.com/) (which you should be), you can utilize their awesome [form handling feature](https://www.netlify.com/docs/form-handling/) for free. Setting this up is super fast and easy, you'll never want to jump through hoops or embed an ugly form again.

For this example, I'll be using a static site made with [Gatsby.js](https://www.gatsbyjs.org/). You can do this with pretty much any static site generator, just make sure you're hosting with Netlify.

<h4><a href="https://gatsby-forms.netlify.com/">Gatsby Site with Forms</a> <small>( <a href="https://github.com/codebushi/gatsby-forms">view source</a> )</small></h4>

Assuming you have Gatsby.js installed on your machine, let's start off by cloning a [Gatsby starter](https://codebushi.com/gatsby-starters/) I've previously made.

```bash
# Create a new Gatsby site with the Forty starter
gatsby new gatsby-forms https://github.com/ChangoMan/gatsby-starter-forty

# Go into the new directory
cd gatsby-forms/

# Start the dev site, browse to http://localhost:8000/
gatsby develop
```

Your new Gatsby site should be up and running, scroll down to the bottom of the page to see the contact form. Since Gatsby is built using React.js, this form area is just simple React component. Open up the file in your text editor of choice, `src/components/Contact.js`

Here's the initial JSX for the form:

```jsx
<form method="post" action="#">
    <div className="field half first">
        <label htmlFor="name">Name</label>
        <input type="text" name="name" id="name" />
    </div>
    <div className="field half">
        <label htmlFor="email">Email</label>
        <input type="text" name="email" id="email" />
    </div>
    <div className="field">
        <label htmlFor="message">Message</label>
        <textarea name="message" id="message" rows="6"></textarea>
    </div>
    <ul className="actions">
        <li><input type="submit" value="Send Message" className="special" /></li>
        <li><input type="reset" value="Clear" /></li>
    </ul>
</form>
```

All we need to do is add a few new attributes to the `<form>` tag:

```jsx
<form name="contact" method="post" data-netlify="true" data-netlify-honeypot="bot-field">
    <input type="hidden" name="bot-field" />
    ...
</form>
```

That's it! This is the most minimal way to get form submissions working. Netlify's bots will automatically detect the attribute `data-netlify="true"` when you deploy, and process the form for you. The `data-netlify-honeypot="bot-field"` is optional, but it's for anti-spam and easy enough to include. If a spam bot fills out the hidden `bot-field` input, then the form won't be submitted.

Now just create a new GitHub repository with these files and [deploy to Netlify](https://www.netlify.com/docs/continuous-deployment/).

Go to your new site and submit the form. You should get directed to a generic success page and we can look into changing this a bit later. Netlify will have a Forms area in their back-end where you can see the submissions. You can also head into `Settings > Forms > Form notifications` and receive an email alert anytime a new submission comes through. Pretty awesome!

<h4 class="mt-5 mb-3">Caveat for Normal React Apps</h4>

It's worth noting that if you're building a normal React app with something like [Create React App](https://github.com/facebook/create-react-app), there are a few extra steps you'll need to follow. Netlify has a [blog post](https://www.netlify.com/blog/2017/07/20/how-to-integrate-netlifys-form-handling-in-a-react-app/) you can follow for these situations. Since we're using Gatsby to create this example, we get the benefit of a static snapshot and can skip these extra steps.

<h3 class="mt-5 mb-3">Adding a Custom Success Page</h3>

Adding your own success page is pretty straightforward, we'll first need to create a new page in our Gatsby site. Make a new file at `src/pages/success.js` with the following:

```jsx
import React from 'react'
import Helmet from 'react-helmet'

import pic11 from '../assets/images/pic11.jpg'

const Success = (props) => (
    <div>
        <Helmet>
            <title>Success Page</title>
            <meta name="description" content="Success Page" />
        </Helmet>

        <div id="main" className="alt">
            <section id="one">
                <div className="inner">
                    <header className="major">
                        <h1>Success/Thank You Page</h1>
                    </header>
                    <span className="image main"><img src={pic11} alt="" /></span>
                    <p>Thank you for contacting us!</p>
                </div>
            </section>
        </div>

    </div>
)

export default Success
```

Browse over to http://localhost:8000/success and confirm your new page. Next we need to add an `action` to our contact form. Open up `src/components/Contact.js` and replace the `<form>` tag with this:

```jsx
<form name="contact" method="post" action="/success" data-netlify="true" data-netlify-honeypot="bot-field">
```

The `action="/success"` will tell Netlify to use our new page instead of their generic success page. Commit these changes and wait for the new deploy, then fill out your form again. You should be re-directed to the new success page!

Netlify currently allows for 100 free form submissions per month, which is pretty generous and a good starting point for our purposes.

<h4><a href="https://gatsby-forms.netlify.com/">Finished Product</a> <small>( <a href="https://github.com/codebushi/gatsby-forms">view source</a> )</small></h4>
