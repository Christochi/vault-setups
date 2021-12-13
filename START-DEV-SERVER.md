## STEPS TO START THE DEV SERVER
- on the CLI, run `vault server -dev`
- launch a new terminal session (2nd terminal)
- copy and run the `export VAULT_ADDR='http://127.0.0.1:8200'` command from the shell of the first terminal in the new terminal session   
- save the unseal key somewhere in case you need to unseal vault at anytime. for e.g: `echo "put unseal key value" > unseal.txt`
- set the VAULT_TOKEN environment variable value to the generated root token value (`Root Token: ....`) displayed in the shell of the first terminal in the 2nd terminal session: `export VAULT_TOKEN="......"`
- verify server is running by running "vault status"

***To interact with vault, you must provide a valid token. Setting the environment variable is a way to provide token to Vault via CLI***

**Ctrl + c** to shutdown server
