#### Configuring Auth Methods Using the CLI
when interacting with CLI, use the `vault auth <subcommand>`. The subcommands are: enable, disable, list, tune, help
- `enable`: vault auth enable approle (approle is the type of auth method)
- `disable`: vault auth disable approle (approle is the path, specifies the path of the obj not type)
- `list`: vault auth list (easy way to determine which auth method is enabled)
- `tune`: used to modify the configuration of an auth method like modifying the default ttl
- `help`: vault auth help (gives info on how to use the vault auth cmd)

***To see all the info (configuration) related to the role and to know what params (like below) to configure: `vault read auth/approle/role/vault-course`. Use "read" to know params you can configure. You configure by doing a "write"***

```
vault write auth/approle/role/vault-course \
   secret_id_ttl=10m \
   token_num_uses=10 \
   token_ttl=20m \
   token_max_ttl=30m \
   secret_id_num_uses=40
```

#### Steps:
1. enable approle in default path: `vault auth enable approle` or using a custom path: 
```
vault auth enable -path=vault-course approle
```
2. create policy and role for apps and associate role with the policy: 
```
vault write auth/<default or custom path>/<role> token_policies="<policyname>"
```

Example:
```
vault write auth/approle/role/jenkins token_policies="jenkins-role-policy"
```  
3. get role id which will be used to get username: 
```
vault read auth/approle/role/jenkins/role-id
```
4. generate a secret id: 
```
vault write -f auth/approle/role/jenkins/secret-id
```
(-f or -force). The result would be a secret_id and secred_id_accessor. The secret_id is used to obtain a password. It changes everytime its read

5. app authenticates (logs in) with role id and secret id: (step 5-7)
```
vault write auth/approle/login role_id="..." secret_id="..." 
```
6. if the authentication is successful, vault will send back a token to the app