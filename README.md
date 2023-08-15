# How to Use Query and Resolver in GraphQL

This guide will walk you through the steps to set up GraphQL in your project and use queries and resolvers. We will use Apollo Server for a built-in standalone HTTP server and middleware for Express.

## What is Query in GraphQL?

In GraphQL, a query is a way to request data from the server. It allows users to specify the fields they want to retrieve, which helps reduce over-fetching and under-fetching of data. With GraphQL queries, you can retrieve specific data in a single request, making the API more efficient and reducing network traffic.

Queries in GraphQL are analogous to GET requests in RESTful APIs. They are used to fetch data from the server. You define queries using the GraphQL query language, which allows you to specify the structure of the data you want to receive.

For example, suppose you have a GraphQL server that exposes data about users and quotes. You can write a query like this to fetch the username and the content of a quote:

```graphql
query {
  user(id: "123") {
    username
  }
  quote(id: "456") {
    content
  }
}
```

This query will request the server to provide the `username` of the user with ID "123" and the `content` of the quote with ID "456."

## What is Resolver in GraphQL?

Resolvers in GraphQL are functions responsible for fetching the data requested by the client in the query. They provide the logic to resolve the fields specified in the query and return the corresponding data from the server's data source.

When a client sends a query to the server, the server uses resolvers to fetch the data for each field in the query. Each field in the query corresponds to a resolver function that knows how to retrieve the data for that particular field.

Using the previous query example, let's assume the server has the following data for the user and quote:

```json
{
  "user": {
    "id": "123",
    "username": "john_doe"
  },
  "quote": {
    "id": "456",
    "content": "The only way to do great work is to love what you do."
  }
}
```

To resolve the `user` and `quote` fields in the query, the server's resolvers could be implemented like this:

```javascript
const usersData = {
  "123": {
    id: "123",
    username: "john_doe"
  },
};

const quotesData = {
  "456": {
    id: "456",
    content: "The only way to do great work is to love what you do."
  },
};

const resolvers = {
  Query: {
    user: (parent, args) => usersData[args.id],
    quote: (parent, args) => quotesData[args.id],
  },
};
```

In this example, the `user` resolver fetches the user data based on the provided `id`, and the `quote` resolver fetches the quote data based on the provided `id`. The data is then returned to the client as specified in the query.

Resolvers are a crucial part of a GraphQL server as they define how data is fetched and returned in response to the client's queries.

## Step 1: Install GraphQL

To get started, you need to install the `graphql` package using Yarn or npm.

```bash
yarn add graphql
# or
npm install graphql
```

## Step 2: Install Apollo Server

Apollo Server provides an easy way to set up a GraphQL server with various features like plugin support, integration with Apollo Studio, caching, and more.

Install `apollo-server` using Yarn or npm.

```bash
yarn add apollo-server
# or
npm install apollo-server
```

## Step 3: Install Apollo Server Core

Apollo Server Core implements the core logic of Apollo Server and is required for using the playground in your project.

```bash
yarn add apollo-server-core
# or
npm install apollo-server-core
```

## Step 4: Set up Apollo Server

Now that you have installed the necessary packages, you can set up Apollo Server in your project. Below is a basic example of how to do it with Express:

```javascript
const { ApolloServer, gql } = require('apollo-server-express');
const express = require('express');

// Define your GraphQL schema using gql tagged template literal
const typeDefs = gql`
  type Query {
    hello: String
  }
`;

// Define your resolver functions
const resolvers = {
  Query: {
    hello: () => "Hello, GraphQL!",
  },
};

// Create an instance of ApolloServer
const server = new ApolloServer({ typeDefs, resolvers });

// Create an instance of Express server
const app = express();

// Apply ApolloServer as middleware to Express
server.applyMiddleware({ app });

// Start the server
const PORT = 4000;
app.listen(PORT, () => {
  console.log(`Server is running at http://localhost:${PORT}/graphql`);
});
```

## Step 5: Test Your GraphQL API

With the server set up, you can now test your GraphQL API using the Apollo Server Playground. Open your browser and go to `http://localhost:3000/graphql` (assuming you used the default port).

In the playground, you can interact with your API by executing queries and mutations. For our example, you can try running the following query:

```graphql
query {
  hello
}
```

The result should be:

```json
{
  "data": {
    "hello": "Hello, GraphQL!"
  }
}
```

Congratulations! You have successfully set up a basic GraphQL server using Apollo Server with Express and tested it using the Playground.

Feel free to explore more complex queries, mutations, and resolvers as your project grows. Happy coding!
