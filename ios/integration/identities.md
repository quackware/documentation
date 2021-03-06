# Identity

The `LYRIdentity` class represents a synchronized user identity model from an application's own provider to the Layer platform.  Identities can be created via extension of the [authentication handshake](/docs/ios/integration/authentication) or via [Platform API](/docs/platform/).
All `LYRConversation` `participants` and `LYRMessage` `sender` objects are of type `LYRIdentity`, with a minimum of property `userID`.  All additional identity information is synchronized to the client when available.

## Following

UserIDs can be followed to receive identity updates when they change on the platform.  Conversation participants are already implicitly followed and cannot be explicitly unfollowed.  Non-conversation participant identities can be explicitly followed by calling `LYRClient` method `followUserIDs:error:` and unfollowed via `unfollowedUserIDs:error:`.  This is useful for presenting a list of identities a user can begin a new conversation with, without relying on an external user management system.

```objectivec
// Follow a userID to create a queryable `LYRIdentity` object that receives updates via Platform API
NSError *error;
BOOL success = [layerClient followUserIDs:[NSSet setWithObject:@"userID"] error:&error];

// Unfollows a userID to stop receiving identity mutation updates
NSError *error;
BOOL success = [layerClient unfollowUserIDs:[NSSet setWithObject:@"userID"] error:&error];
```

## Querying

`LYRIdentity` conforms to the `LYRQueryable` protocol and in addition to default `LYRPredicateOperator`s is also specially queryable via `LYRPredicateOperatorLike` which allows for wildcard querying.

```objectivec
// Queries for all identities with display name `randomName`
LYRQuery *query = [LYRQuery queryWithQueryableClass:[LYRIdentity class]];
query.predicate = [LYRPredicate predicateWithProperty:@"displayName" predicateOperator:LYRPredicateOperatorIsEqualTo value:@"randomName"];

// Queries for all identities with display name beginning with `rando`
LYRQuery *query = [LYRQuery queryWithQueryableClass:[LYRIdentity class]];
query.predicate = [LYRPredicate predicateWithProperty:@"displayName" predicateOperator:LYRPredicateOperatorLike value:@"rando%"];

// Queries for all identities that start with `random`, any character, and end with `ame`
LYRQuery *query = [LYRQuery queryWithQueryableClass:[LYRIdentity class]];
query.predicate = [LYRPredicate predicateWithProperty:@"displayName" predicateOperator:LYRPredicateOperatorLike value:@"random_ame"];
```
