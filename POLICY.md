#### Managing Policies Using The CLI 
`vault policy subcommand`. The subcmds are:
- **delete:** `vault policy delete [policy-name]`  
- **fmt:** `vault policy fmt [filename]` 
- **list:** `vault policy list [policy-name]`
- **read:** `vault policy read [policy-name]`
- **write:** `vault policy write [policy-name] [filename or filepath]`

`Example: vault policy write unseal policy.hcl`

***path or directory to place policy is optional***

***it is optional for policy names to have a `.hcl` extension but it is best practice to use it***

#### Vault Policies - Capabilities
The available capabilities:
- **create:** POST/PUT (If the key does not exist, create)
- **read:** GET
- **update:** POST/PUT (if the key exists and you want to replace/update it)
- **delete:** DELETE
- **list:** LIST
- **sudo:** allows access to paths that are root-protected
- **deny:** disallows access regardless of any defined capabilities

```
path "database/creds/dev-db01" {
   capabilities = ["read"]
}

path "kv/apps/dev-app01" {
   capabilities = ["create", "read", "update", "delete"]
}
```

#### Working With Policies
- create a new token and associate it with a specific policy use this cmd: 
```
vault token create -policy="policy-name"
```
- login: `vault login [token]`
- to know the policy attached to a token: `vault token lookup`

 #### ACL Rules Format - KV V2
 Writing and reading versions are prefixed with the `data/` path.
 
 This policy that worked for the version 1 kv:
 ```
 path "secret/dev/team-1/*" {
  capabilities = ["create", "update", "read"]
}
```

Should be changed to:
```
path "secret/data/dev/team-1/*" {
  capabilities = ["create", "update", "read"]
}
```

