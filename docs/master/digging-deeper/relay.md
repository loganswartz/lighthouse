# Relay

## Cursor Connection

Relay requires a particular kind of pagination which is the [Cursor Connection](https://facebook.github.io/relay/graphql/connections.htm)
To get a relay-compatible connection on a root query field, use the [@paginate](../api-reference/directives.md#paginate)
directive with the pagination type `CONNECTION`.

> Lighthouse does not support actual cursor-based pagination as of now, see https://github.com/nuwave/lighthouse/issues/311 for details.
> Under the hood, the "cursor" is decoded into a page offset.

```graphql
type Query {
  users: [User!]! @paginate(type: CONNECTION)
}
```

This automatically converts the type definition into a relay connection and constructs
the appropriate queries via the underlying Eloquent model.

Connections can also be used for subfields of a type, given they are defined as a HasMany-Relationship
in Eloquent. Use the [@hasMany](../api-reference/directives.md#hasmany) directive.

```graphql
type User {
  name: String
  posts: [Post!]! @hasMany(type: CONNECTION)
}
```

## Global Object Identification

You may rebind the `Nuwave\Lighthouse\Support\Contracts\GlobalId` interface to add your
own mechanism of encoding/decoding global IDs.

[Global Object Identification](https://relay.dev/graphql/objectidentification.htm)

Directives:

- [@node](../api-reference/directives.md#node)
- [@globalId](../api-reference/directives.md#globalid)

## Input Object Mutations

To follow the principle of using a single field argument called `input`, use the [@spread](../api-reference/directives.md#spread) directive.

```graphql
type Mutation {
  introduceShip(input: IntroduceShipInput! @spread): IntroduceShipPayload!
}
```
