## Authenticate User

This message is a request to the plugin to authenticate and authorize a user, along with the user credentials and all known
`<authConfig />`  and `<roleConfig />`configured for the plugin.

<p class='request-name-heading'>Request name</p>

`go.cd.authorization.authenticate-user`

<p class='request-body-heading'>Request body</p>

> The plugin will receive the following JSON body for password based authorization —

```json
{
  "credentials" : {
    "username": "jdoe",
    "password": "secret"
  },
  "auth_configs": [{
    "id": "internal_ldap",
    "configuration": {
      "url": "ldap://ldap1.example.com"
    }
  },
  {
    "id": "external_ldap",
    "configuration": {
      "url": "ldap://ldap2.example.com"
    }
  }],
  "role_configs": [{
    "name": "admin",
    "auth_config_id": "internal_ldap",
    "configuration": {
      "memberOf": "ou=some-value"
    }
  },
  {
    "name": "view",
    "auth_config_id": "external_ldap",
    "configuration": {
      "memberOf": "ou=some-value"
    }
  }]
}
```

<p class='attributes-table-follows'></p>

| Key            | Type     | Description                                                                   |
|----------------|----------|-------------------------------------------------------------------------------|
| `credentials`  | `Object` | For a plugin which supports password based authentication, this key should contain the 'username' and 'password' provided by the user at the time of login. |
| `auth_configs` | `Object` | This key contains list of `<authconfig>` configured for the plugin. |
| `role_configs` | `Object` | This key contains list of `<roleconfig>` configured for the plugin. |

<p class='response-code-heading'>Response Body</p>

> An example response body —

```json
{
  "user": {
    "username": "jdoe",
    "display_name": "John Doe",
    "email_id": "jdoe@example.com"
  },
  "roles": ["blackbird", "spacetiger"]
}
```

The response body will contain the following JSON elements:

<p class='attributes-table-follows'></p>

| Key                   | Type      | Description |
| --------------------- | --------- | ----------- |
| `user`                | `Object`  | This json object contains user details like `username`, `display_name` and `email_id`. |
| `roles`               | `Array`   | Array of roles associated with authenticated user. |

The plugin is expected to return status `200` if it can understand the request.
