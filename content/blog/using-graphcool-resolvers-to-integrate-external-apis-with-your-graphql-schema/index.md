---
title: Using Graphcool resolvers to integrate external APIs with your GraphQL schema
date: 2018-09-25
---

I’ve used [Graphcool](https://graph.cool) on a few personal projects. It is a great tool to bootstrap a GraphQL backend with minimum setup:

> Open-source and self-hosted backend-as-a-service to develop serverless GraphQL backends. The Graphcool Framework offers an opionated backend setup, including GraphQL database mapping, subscriptions & permission system.

Within a few minutes setting your schema, you get a database backed with a GraphQL API and all possible queries and mutations. Pretty cool.

Last week I was developing an app that must be integrated with the [Google Places API](https://developers.google.com/places/web-service/intro). Initially, I had to just parse a place using a place ID.

The Places documentation is pretty straightforward, and I was able to make a quick successful test using the provided endpoint:

    https://maps.googleapis.com/maps/api/place/details/json?placeid=**PLACE_ID**&key=**YOUR_API_KEY**

Cool, but if I just query this on my app using an HTTP client like [axios](https://github.com/axios/axios) or something like that, I’ll lose all the advantages of using a GraphQL API. I use Apollo, for example, and it provides me with all the cool stuff, like automatic caching and a declarative API.

So, what if Graphcool could do this query for me and handle me back the data as if it were from my own API? You can achieve this using **Resolver functions.**

> With a resolver you can provide custom queries and mutations that are powered by a function that you provide.

![](./images/1.png)

## Setting up a Resolver function

Create a new function and select **Resolver **as the type. Then, you will need to write two files — directly on the Graphcool UI:

### Resolver SDL

**Specifies how you will extend your schema.**

In my case, I’m adding GooglePlace as a new type in my schema. It has just a payload field, which returns Json data.

The query to parse a GooglePlace accepts one argument: id

My file looks like this:

<iframe src="https://gist.github.com/dlbnco/d6bebc0c1c74c071c27e4c9c416d8c80.pibb"></iframe>

### Inline code

**The function that will be called when you call the query you just described**

In my case, I will just call the Google Places API using the provided id and my API key, and return the data as the payload field on my GooglePlace type:

<iframe src="https://gist.github.com/dlbnco/335faca3cd7e7843b65514f3a2f5139b.pibb"></iframe>

**Done!**🎉

Now I can query the Google Places API directly through my Graphcool backend using GraphQL:

![](./images/2.png)

For more info, check out the [Graphcool documentation on Resolver functions](https://www.graph.cool/docs/reference/functions/resolvers-su6wu3yoo2).

👋
