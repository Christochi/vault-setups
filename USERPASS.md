#### Configuring Auth Methods Using the CLI
when interacting with CLI, use the "vault auth subcommand". The subcommands are: enable, disable, list, tune, help
- `enable`: vault auth enable approle (approle is the type of auth method)
- `disable`: vault auth disable approle (approle is the path, specifies the path of the obj not type)
- `list`: vault auth list (easy way to determine which auth method is enabled)
- `tune`: used to modify the configuration of an auth method like modifying the default ttl
- `help`: vault auth help (gives info on how to use the vault auth cmd)

#### Steps:
- enable userpass: `vault auth enable userpass`
- configure username and password: 
```
vault write auth/userpass/users/frank password=vault policies=....
```
- login using userpass credentials: 
```
vault login -method=userpass username=frank password=vault
```

to look at the users that are associated with your userpass method: 
`vault list auth/userpass/users`

to read the configuration of a particular user: `vault read auth/userpass/users/frank`

