[назад](../README.md)

# Задание 5. Проектирование GraphQL API

>Спроектируйте GraphQL на основании существующего контракта сервиса

Результат проектирования схемы GraphQL API:
```graphql
schema {
  query: Query
}

type Query {
  client(id: String!): Client
}

type Client {
  id: String
  name: String
  age: Int

  documents: [Document]
  relatives: [Relative]
}

type Document {
  id: String
  type: String
  number: String
  issueDate: String
  expiryDate: String
}

type Relative {
  id: String
  relationType: String
  name: String
  age: Int
}

```

[назад](../README.md)