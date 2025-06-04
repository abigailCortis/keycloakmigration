---
author: klg71
layout: post
title:  "Client Migrations"
date:   2020-07-03 12:22:20 +0200
permalink: /migrations/client/
---
# Client Migrations
All migrations referring to the client resource.
## addSimpleClient
Simple command to add a client to keycloak.

### Parameters
| Parameter Name               | Type                 | Optional? | Default Value    |
|------------------------------|----------------------|-----------|------------------|
| realm                        | String               | Yes       |                  |
| clientId                     | String               | No        |                  |
| enabled                      | Boolean              | Yes       | true             |
| attributes                   | Map< String, String> | Yes       | empty            |
| protocol                     | String               | Yes       | "openid-connect" |
| secret                       | String               | Yes       |                  |
| publicClient                 | Boolean              | Yes       | true             |
| redirectUris                 | List< String>        | Yes       | empty            |
| authorizationServicesEnabled | Boolean              | Yes       | false            |
| serviceAccountsEnabled       | Boolean              | Yes       | true             |

### Example
```yaml
    id: add-simple-client
    author: klg71
    changes:
    - addSimpleClient:
        realm: master
        clientId: test
```

### Further Enhancements
- add more fields

## deleteClient
Delete a client in keycloak

### Parameters
| Parameter Name | Type   | Optional? |
|----------------|--------|-----------|
| realm          | String | Yes       |
| clientId       | String | No        |

### Example
```yaml
    id: delete-client
    author: klg71
    changes:
    - deleteClient:
        realm: master
        clientId: test
```

## importClient
Imports a client using the json representation.

### Parameters
| Parameter Name                   | Type    | Optional? | Default Value |
|----------------------------------|---------|-----------|---------------|
| realm                            | String  | Yes       |               |
| clientRepresentationJsonFilename | String  | No        |               |
| relativeToFile                   | Boolean | Yes       | true          |

### Example
```yaml
    id: import-client
    author: klg71
    changes:
    - importClient:
          realm: master
          clientRepresentationJsonFilename: client.json
          relativeToFile: true
```

## updateClient
Update a client

### Parameters
| Parameter Name               | Type                | Optional? | Default Value |
|------------------------------|---------------------|-----------|---------------|
| realm                        | String              | Yes       |               |
| clientId                     | String              | No        |               |
| name                         | String              | Yes       | _no change_   |
| description                  | String              | Yes       | _no change_   |
| surrogateAuthRequired        | Boolean             | Yes       | _no change_   |
| enabled                      | Boolean             | Yes       | _no change_   |
| alwaysDisplayInConsole       | Boolean             | Yes       | _no change_   |
| clientAuthenticatorType      | String              | Yes       | _no change_   |
| attributes                   | Map<String, String> | Yes       | _no change_   |
| protocol                     | String              | Yes       | _no change_   |
| redirectUris                 | List< String>       | Yes       | _no change_   |
| notBefore                    | Boolean             | Yes       | _no change_   |
| bearerOnly                   | Boolean             | Yes       | _no change_   |
| consentRequired              | Boolean             | Yes       | _no change_   |
| directAccessGrantEnabled     | Boolean             | Yes       | _no change_   |
| implicitFlowEnabled          | Boolean             | Yes       | _no change_   |
| standardFlowEnabled          | Boolean             | Yes       | _no change_   |
| adminUrl                     | String              | Yes       | _no change_   |
| baseUrl                      | String              | Yes       | _no change_   |
| rootUrl                      | String              | Yes       | _no change_   |
| publicClient                 | Boolean             | Yes       | _no change_   |
| frontchannelLogout           | Boolean             | Yes       | _no change_   |
| serviceAccountsEnabled       | Boolean             | Yes       | _no change_   |
| webOrigins                   | List< String>       | Yes       | _no change_   |
| fullScopeAllowed             | Boolean             | Yes       | _no change_   |
| nodeReRegistrationTimeout    | Int                 | Yes       | _no change_   |
| authorizationServicesEnabled | Boolean             | Yes       | _no change_   |

### Example
```yaml
    id: update-client
    author: klg71
    changes:
    - updateClient:
        realm: master
        clientId: testClient
        redirectUris: 
            - http://localhost:8080
            - https://www.example.com
```

## assignRoleToClient
Assigns a realm- or client-role(if roleClientId is set) to a service account of a client.

### Parameters
| Parameter Name | Type   | Optional? | Default Value |
|----------------|--------|-----------|---------------|
| realm          | String | Yes       |               |
| clientId       | String | No        |               |
| role           | String | No        |               |
| roleClientId   | String | Yes       | realmRole     |

### Example
```yaml
    id: add-client-roles
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testClientRoles
      - updateClient:
          clientId: testClientRoles
          serviceAccountsEnabled: true
          publicClient: false
      - assignRoleToClient:
          clientId: testClientRoles
          role: query-users
          roleClientId: realm-management
```

## addRoleScopeMapping
Adds a realm- or client-role(if roleClientId is set) to the scope mappings of a client.

See https://www.keycloak.org/docs/latest/server_admin/#_role_scope_mappings

### Parameters
| Parameter Name | Type   | Optional? | Default Value |
|----------------|--------|-----------|---------------|
| realm          | String | Yes       |               |
| clientId       | String | No        |               |
| role           | String | No        |               |
| roleClientId   | String | Yes       | realmRole     |

### Example
```yaml
    id: add-client-role-mapping
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testClientRoleScopeMappings
      - addRole:
          name: scope-mapping-role
      - updateClient:
          clientId: testClientRoleScopeMappings
          fullScopeAllowed: false
      - addRoleScopeMapping:
          clientId: testClientRoleScopeMappings
          role: scope-mapping-role
      - addRoleScopeMapping:
          clientId: testClientRoleScopeMappings
          role: query-users
          roleClientId: realm-management
```

## deleteRoleScopeMapping
Deletes a realm- or client-role(if roleClientId is set) from the scope mappings of a client.

See https://www.keycloak.org/docs/latest/server_admin/#_role_scope_mappings

### Parameters
| Parameter Name | Type   | Optional? | Default Value |
|----------------|--------|-----------|---------------|
| realm          | String | Yes       |               |
| role           | String | No        |               |
| clientId       | String | No        |               |
| roleClientId   | String | Yes       | realmRole     |

### Example
```yaml
    id: add-client-role-mapping
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testClientRoleScopeMappings
      - addRole:
          name: scope-mapping-role
      - updateClient:
          clientId: testClientRoleScopeMappings
          fullScopeAllowed: false
      - addRoleScopeMapping:
          clientId: testClientRoleScopeMappings
          role: query-users
          roleClientId: realm-management
      - deleteRoleScopeMapping:
          clientId: testClientRoleScopeMappings
          role: query-users
          roleClientId: realm-management
```

## addClientMapper
Adds a full configurable clientmapper, throws error if client or realm doesn't exist or mapper with same name already exists

> Only use this action if you can't find a convenient method to add the mapper below

### Parameters
| Parameter Name | Type               | Optional? | Default Value    |   |
|----------------|--------------------|-----------|------------------|---|
| realm          | String             | Yes       |                  |   |
| clientId       | String             | No        |                  |   |
| name           | String             | No        |                  |   |
| config         | Map<String,String> | No        |                  |   |
| protocolMapper | String             | No        |                  |   |
| protocol       | String             | Yes       | "openid-connect" | ß |

### Example:
```yaml
    id: add-client-mappers
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testMappers
      - addClientMapper:
          clientId: testMappers
          name: testPropertyMapper
          protocolMapper: oidc-usermodel-property-mapper
          config:
            access.token.claim: true
            id.token.claim: false
            userinfo.token.claim: false
            claim.name: customPropertyMapper
            jsonType.label: String
            user.attribute: UserModel.getEmail()
```

## deleteClientMapper
deletes a client mapper

### Parameters
| Parameter Name | Type   | Optional? |
|----------------|--------|-----------|
| realm          | String | Yes       |
| clientId       | String | No        |
| name           | String | No        |

### Example:
```yaml
    id: add-client-mappers
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testMappers
      - addClientMapper:
          clientId: testMappers
          name: testPropertyMapper
          protocolMapper: oidc-usermodel-property-mapper
          config:
            access.token.claim: true
            id.token.claim: false
            userinfo.token.claim: false
            claim.name: customPropertyMapper
            jsonType.label: String
            user.attribute: UserModel.getEmail()
      - deleteClientMapper:
          clientId: testMappers
          name: testPropertyMapper
```

## addClientAudienceMapper
adds a audience clientmapper, throws error if client or realm doesn't exist or mapper with same name already exists

### Parameters
| Parameter Name   | Type    | Optional? | Default Value |
|------------------|---------|-----------|---------------|
| realm            | String  | Yes       |               |
| clientId         | String  | No        |               |
| name             | String  | No        |               |
| addToIdToken     | Boolean | Yes       | true          |
| addToAccessToken | Boolean | Yes       | true          |
| clientAudience   | String  | Yes       | ""            |
| customAudience   | String  | Yes       | ""            |

### Example:
```yaml
    id: add-client-mappers
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testMappers
      - addClientAudienceMapper:
          clientId: testMappers
          name: audienceMapper
          addToIdToken: false
          clientAudience: testMappers
          customAudience: completlyCustom
```

## addClientGroupMembershipMapper
adds a group-membership clientmapper, throws error if client or realm doesn't exist or mapper with same name already exists

### Parameters
| Parameter Name   | Type    | Optional? | Default Value       |
|------------------|---------|-----------|---------------------|
| realm            | String  | Yes       |                     |
| clientId         | String  | No        |                     |
| name             | String  | No        |                     |
| addToIdToken     | Boolean | Yes       | true                |
| addToAccessToken | Boolean | Yes       | true                |
| addToUserInfo    | Boolean | Yes       | true                |
| fullGroupPath    | Boolean | Yes       | true                |
| claimName        | String? | Yes       | << name parameter>> |

### Example:
```yaml
    id: add-client-mappers
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testMappers
      - addClientGroupMembershipMapper:
          clientId: testMappers
          name: groupMembership
          addToAccessToken: false
          claimName: groupClaim
```

## addClientUserAttributeMapper
adds a user-attribute clientmapper, throws error if client or realm doesn't exist or mapper with same name already exists

### Parameters
- realm: String, optional
- clientId: String, not optional
- name: String, not optional
- userAttribute: String, not optional
- addToIdToken: Boolean , optional, default = true,
- addToAccessToken: Boolean, optional, default = true,
- addToUserInfo: Boolean, optional, default = true,
- claimName: String?, optional, default = << name parameter>>
- multivalued: Boolean, optional, default = false,
- aggregateAttributeValues: Boolean, optional, default = true

### Example:
```yaml
    id: add-client-mappers
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testMappers
      - addClientUserAttributeMapper:
          clientId: testMappers
          name: userAttribute
          userAttribute: testAttribute
          addToUserInfo: false
```

## addClientUserRealmRoleMapper
adds a user-realm-role clientmapper, throws error if client or realm doesn't exist or mapper with same name already exists

### Parameters
- realm: String, optional
- clientId: String, not optional
- name: String, not optional
- addToIdToken: Boolean , optional, default = true,
- addToAccessToken: Boolean, optional, default = true,
- addToUserInfo: Boolean, optional, default = true,
- claimName: String?, optional, default = << name parameter>>
- prefix: String, optional, default = ""

### Example:
```yaml
    id: add-client-mappers
    author: klg71
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: testMappers
      - addClientUserRealmRoleMapper:
          clientId: testMappers
          name: userRealmRole
          prefix: rolePrefix
```

## importClientAuthorization
Imports authorization configuration using the JSON representation

### Parameters
- realm: String, optional
- clientId: String, not optional
- authorizationRepresentationJsonFilename: String, not optional
- relativeToFile: Boolean, optional, default=true

### Example:
```yaml
    id: import-client-authorization
    author: devtobi
    realm: integ-test
    changes:
      - addSimpleClient:
          clientId: test
          serviceAccountsEnabled: true
          authorizationServicesEnabled: true
      - importClientAuthorization:
          clientId: test
          authorizationRepresentationJsonFilename: authorization.json
          relativeToFile: true
```

