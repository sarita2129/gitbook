---
description: It's the progressive web old sport.
---

# Day 05

What we covered today:

* [Gatsby](https://github.com/wofockham/wdi-30/tree/master/13-advanced/gatsby)
* [Surge](https://surge.sh/)

Warmup

* [Wordy Calculator](https://github.com/Yiannimoustakas/wdi30-homework/tree/master/warmups/week10/day05_wordy_calculator)

## Gatsby

#### What is Gatsby?

Gatsby is a [React](https://reactjs.org/docs/getting-started.html)-based, [GraphQL](https://graphql.org/learn/) powered, [static site generator](https://www.netlify.com/blog/2017/05/25/top-ten-static-site-generators-of-2017/). What does that even mean?  Well, it weaves together the best parts of React, [webpack](https://webpack.js.org/concepts/), [react-router](https://reacttraining.com/react-router/core/guides/philosophy), GraphQL, and other front-end tools in to one very enjoyable developer experience. 

**Installfest:**

```bash
npm install -g gatsby
```

Start a new project:

```bash
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

To start the development server:

```bash
gatsby develop
```

## Surge

#### What is Surge sh?

Shipping web projects should be fast, easy, and low risk. _Surge_ is static web publishing for Front-End Developers, right from the CLI.

To deploy to Surge:

First build your static html pages in your public folder. To do this run:

```bash
npm run build
```

Make sure your html files are present in your public folder:

```bash
tree public/
```

To deploy simply type:

```bash
surge public
```



