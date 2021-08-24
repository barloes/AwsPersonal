# Graphql AWS Basics 
- https://docs.amplify.aws/cli/graphql-transformer/connection
- https://docs.amplify.aws/cli/graphql-transformer/key

1. one-one relationship
```
type Person @model
{ 
  id: ID!

  walletId: ID!
  name: [Name]@connection(keyName:"walletByPerson",fields:["id"])

}

type Name @model
@key(name:"walletByPerson", fields:["id"])
{
  
  id:ID!

  personId: ID!
  Person:[Person] @connection(fields:["id"])

}
```

2. one-one relationship with Name having composite key(firstname,lastname) 
- composite key only works when there is 1-1 relationship 
```
type Person @model
{ 
  id: ID!

  nameId: ID!
  name: [Name]@connection(keyName:"nameByPerson",fields:["nameId"])

}

type Name @model
@key(fields:["firstname","lastname"])
@key(name:"nameByPerson", fields:["id"])
{
  
  id:ID!
  Person:[Person] @connection(fields:["id"])

  firstname: ID!
  lastname: ID!
}

```

3. one-many relationship 
- Person has many Wallets -> Wallets will point to the same person
- requires personId in Wallet model to connect back to Person

```
type Person @model
{ 
  id: ID!

  wallets: [Wallet]@connection(keyName:"walletByPerson",fields:["id"])

}

type Wallet @model
@key(name:"walletByPerson", fields:["id"])
{
  id:ID!

  personId: ID!
  Person:[Person] @connection(fields:["id"])
}

```