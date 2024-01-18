# API Routes

## What are API Routes?

- API - Application Programming Interface

- REST API = Representational State Transfer (a specific structure of web APIs)

API: We have a web server that exposes certain URLs (APIs) that can accept data and then send back a response (json is the most common format of data exchange).

API routes - special kind of URLs which we can add to a next.js app - which are about getting data, using data, maybe storing data in some database, and sending back data in any form of your choice.

API routes (next.js feature) is a feature that allows us to build a REST API as part of our next.js app. It also allows us to support URLs / paths after our domain, then also accepting different kind of request methods, then depending on which http method was used for which path, different actions could be triggered. For example, for a POST request we might expect some data to be sent along with that incoming request, then when that request hits the API, we extract that data from the request and store it in a database. If a GET request reaches the URL, we might want tof etch data from a database (as raw data in JSON format).

These requests are sent and triggered by JavaScript code (Ajax).

## Writing a API Route

API Routes (next.js feature).

Add API endpoints to next.js app directory: `pages/api`

`api` - special folder (it has to be called like this) that is recognized by next.js, any file within this `api` folder will be treated in a special way.

In this `api` folder, we can add pages just as we can add them anywhere else in the `pages` folder.

`pages/api/feedback.js` - this will create a path we can send requests to.

Inside of any file from the `api` folder, we don't export a react component, instead we create and export a `handler` function that will receive 2 parameters: a request object `req` and a response object `res`.

In this `handler` function we can execute server-side code - any code we write here will not end up in any client-side bundle, code from here will not be exposed to visitors of our webpage, just as code from `getStaticProps` and `getServerSideProps`.

This function is executed by next.js for incoming requests sent to `/api/feedback`. In can write node.js code here.

We can send back a resonse with help of the `res` object that contains special methods for sending back response and to configure that response.

```js
export default function handler(req, res) {
  res.status(200).json({ message: "This works!" });
}
```

This line of code from the `handler` function from `/pages/api/feedback.js` will be executed by next.js when we send a request to `/api/feedback`, and this code will then send back a response.

If we make a request to `/api/feedback` by directly accessing the url in the browser, then we get on the page: `{"message":"This World!"}` - but this is not how we do this requests - we can check more about this in the network tab of devtools, for example the reponse itself. In Headers tab of network, the Content-Type is set to application/json. So this is a special kind of page, files from `pages/api/file.js` allows us to run our own server side code, and to add our own app focused API, we can add features like newsletter, user feedback submission, etc.

## Forms

Submitting a form suppose to send some data to a database.

After form submit, we send request to an API route, then in that api route we can connect to a database - security reasons - in api routes we can safely talk to a database because it is not exposed to the frontend.

**Any code that "talks" to a database belongs onto a server.** The frontend only sends HTTP requests to that server by exposing a REST API on that server.

Alternatives to REST APIs: SDKs - Firebase or MongoDB Stitch. This alternatives suppose to talk to a web API which then in turn runs the databse queries.

## Parsing The Incoming Request & Executing Server-side Code

Inside the `handler` function from the `api` folder, first we have to find which type of request is triggering the API route, and we do this using the `req` object parameter that we get automatically by next.js

The `req` object contains information about the incoming request.

`req.method` this method contains the http method used.

`req.body` contains the actual data, this is the already parsed body of the incoming request, next.js will automatically parse the incoming request body for us.

```js
import fs from "fs";
import path from "path";

export default function handler(req, res) {
  if (req.method === "POST") {
    const email = req.body.email;
    const feedbackText = req.bodt.text;

    const newFeeback = {
      id: new Date().toISOString(),
      email: email,
      text: feedbackText,
    };

    // store that in a database or in a file
    const filePath = path.join(process.cwd(), "data", feedback.json);
    const fileData = fs.readFileSync(filePath);
    const data = JSON.parse(fileData);
    data.push(newFeeback);
    fs.writeFileSync(filePath, JSON.stringify(data));
    res.status(201).json({ message: "Success!", feedback: newFeeback });
  } else {
    res.status(200).json({ message: "Hello World!" });
  }
}
```

This is how we can extract data for the incoming request if it's a `POST` request, then we can store it in a database using node.js code.

Server-side code added to a API route - connecting frontend code `pages/index.js` to backend code `pages/api/feedback.js` - by sending request from the frontend to the API route.

API Route features: looking into a request, extracting request body data, running server-side code, sending a response.

## Sending Requests To API Routes

```js
function submitFormHandler(event) {
  // ...

  fetch("/api/feedback", {
    method: "POST",
    body: JSON.stringify(reqBody),
    headers: {
      "Content-Type": "application/json",
    },
  })
    .then((response) => response.json())
    .then((data) => console.log(data));
}
```

With API routes, we don't have to build a separate api as a separate project, instead we can easily add API routes to the website as part of the project.

## Using API Routes To Get Data

`"GET"` requests

```js
// pages/api/feedback.js
import fs from "fs";
import path from "path";

function buildFeebackPath() {
  return path.join(process.cwd(), "data", "feedback.json");
}

function extractFeedback(filePath) {
  const fileData = fs.readFileSync(filePath);
  const data = JSON.parse(fileData);
  return data;
}

export default function handler(req, res) {
  // ...

  if (req.method === "GET") {
    const filePath = buildFeebackPath();
    const data = extractFeedback(filePath);

    res.status(200).json({ feedback: data });
  }
}
```

```js
function loadFeedbackHandler() {
  fetch("/api/feedback")
    .then((response) => response.json())
    .then((data) => {
      console.log(data.feedback);
    });
}
```

## Using API Routes For Pre-Rendering Pages

How to go from `your-website.com/api/feedback` to `your-website.com/feedback` but that renders the data from the "GET" request on the page.

We can also use `getStaticProps` with **API routes** if we want to pre-render the page and pre-fetch the data for pre-rendering.

**Note:** `fetch` inside `getStaticProps` is ok with External APIs. You should NOT use `fetch` inside `getStaticProps` or `getServerSideProps` to talk to your own API. Instead, considering that this is all part of one project, therefore all served by one server, write node.js logic directly inside `getStaticProps`, like importing logic from the `feedback.js` api route, instead of fetching. Code from `api/feedback.js` and code from `getStaticProps` functions are both not included in the final client-side bundle.

```js
import { buildFeebackPath, extractFeedback } from "../api/feedback";

export default function FeedbackPage(props) {
  return (
    <ul>
      {props.feedbackItems.map((item) => (
        <li key={item.id}>{item.text}</li>
      ))}
    </ul>
  );
}

export async function getStaticProps() {
  const filePath = buildFeebackPath();
  const data = extractFeedback(filePath);

  return {
    props: {
      feedbackItems: data,
    },
  };
}
```

So we do the server-side logic inthe the `getStaticProps` function by importing from `/pages/api/feedback.js`, instead of usign `fetch` and sending a request to our own api route.

When working with your own api rotues and when requiring them in your regular pages, you should not send the http request to them, but instead leverage the fact that it's all running on the same computer (server).

## Creating & Using Dynamic API Routes

Fetch single peace of data for a specific feedback item `your-page.com/feedback/id`

`/pages/api/[feedbackId].js` dynamic api route (works in the same way)

Dynamic routes work for all request methods, not just GET.

- `req.query` for accessing query parameters

```js
// pages/api/[feedbackId].js
import { buildFeebackPath, extractFeedback } from "./feedback";

export default function handler(req, res) {
  const feedbackId = req.query.feedbackId;

  const filePath = buildFeebackPath();
  const data = extractFeedback(filePath);

  const selectedFeedback = data.find((feedback) => feedback.id === feedbackId);

  res.status(200).json({ feedback: selectedFeedback });
}
```

Send request to the dynamic api route:

```js
import { buildFeebackPath, extractFeedback } from "../api/feedback";
import { useState } from "react";

export default function FeedbackPage(props) {
  const [feedbackData, setFeedbackData] = useState();

  function loadFeedbackHandler(id) {
    // api/feedback-id
    fetch(`/api/${id}`)
      .then((res) => res.json())
      .then((data) => setFeedbackData(data.feedback));
  }

  return (
    <>
      {feedbackData && <p>{feedbackData.email}</p>}
      <ul>
        {props.feedbackItems.map((item) => (
          <li key={item.id}>
            {item.text}
            <button onClick={loadFeedbackHandler.bind(null, item.id)}>
              Show Details
            </button>
          </li>
        ))}
      </ul>
    </>
  );
}

export async function getStaticProps() {
  const filePath = buildFeebackPath();
  const data = extractFeedback(filePath);

  return {
    props: {
      feedbackItems: data,
    },
  };
}
```

Consider the above example with `bind`

Send requests to dynamic api routes & set up dynamic api routes to execute server side code for dynamic pieces of data encoded in the path.

## Exploring Different Ways Of Structuring API Route Files

- `api/[...feedbackId].js` we can have catch-all dynamic api routes - requests to `/api/some-value/more-segments`

- This `link.com/api/feedback` will trigger `api/feedback.js` instead of `api/[id].js`, so next,js will prioritize `api/feedback.js`

- `api/feedback.js` === `api/feedback/index.js`

- `api/feedback/[id].js` for `/api/feedback/some-value`

This kind of works the same as pages routes.

## Project

The `api` route is hosted by the same server as the whole project, the same domain, that's why we can use an absolute path in `fetch('/api/feedback)` when fetching data.

a cluster is a server which will then later host your database.

mongoDB driver - connecting and talking to mongoDB

```
npm install mongodb --save
```
