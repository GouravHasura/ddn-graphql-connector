# Building a Supergraph with DDN and a GraphQL Connector

This guide walks you through setting up a supergraph using the Hasura DDN CLI, connecting it to a GraphQL endpoint, customizing the connector configuration, introspecting the schema, and building the supergraph locally.

---

## Prerequisites

- [Hasura DDN CLI installed](https://hasura.io/docs/3.0/ddn/getting-started/)
- Access to a GraphQL endpoint (with admin secret if needed)
- Docker installed and running

---

## Step 1: Initialize a New Supergraph Project

```bash
ddn supergraph init my-project && cd my-project
```

---

## Step 2: Initialize a GraphQL Connector

Inside the project directory:

```bash
ddn connector init my_graphql -i
```

You will be prompted:

- **Hub Connector:** `hasura/graphql`
- **GRAPHQL_ENDPOINT:** Provide your GraphQL API endpoint  
  (example: `https://master-koala-77.hasura.app/v1/graphql`)


---

## Step 3: Customize Connector Configuration for Headers

You can edit the configuration to include headers like `X-Hasura-Admin-Secret` by updating:

File path: `my-project/app/connector/my_graphql/configuration.json`

Example:

```json
{
  "$schema": "configuration.schema.json",
  "introspection": {
    "endpoint": {
      "valueFromEnv": "GRAPHQL_ENDPOINT"
    },
    "headers": {
      "X-Hasura-Admin-Secret": {
        "value": "<SECRET>"
      },
      "Content-Type": {
        "value": "application/json"
      }
    }
  },
  "execution": {
    "endpoint": {
      "valueFromEnv": "GRAPHQL_ENDPOINT"
    },
    "headers": {
      "Content-Type": {
        "value": "application/json"
      }
    }
  }
}
```

Replace `<SECRET>` with your actual Hasura Admin Secret.

---

## Step 4: Introspect the Connector

Now introspect the connector to fetch the GraphQL schema:

```bash
ddn connector introspect my_graphql
```


> After introspection, the connector is ready to pull models, commands, and relationships.

---

## Step 5: Add Models, Commands, and Relationships

Import schema entities into your subgraph:

```bash
ddn model add my_graphql "*"
ddn command add my_graphql "*"
ddn relationship add my_graphql "*"
```

---

## Step 6: Build the Supergraph 

Once everything is added, build the supergraph locally:

```bash
ddn supergraph build create
```
