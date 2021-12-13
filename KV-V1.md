#### List all Secret Engines enabled in your Vault:
`vault secrets list`

#### List all enabled Secrets Engines with Detailed Output:
`vault secrets list -detailed`

When running the kv secrets backend non-versioned, only the most recently written value for a key will be preserved.

#### Enable KV V1 Secret Engine:
`vault secrets enable kv` cmd will mount the kv store in the default i.e kv

#### Enable KV V1 Secret Engine with a custom path:
`vault secrets enable -path=foobar kv`

#### KV V1 sub-commands:
put, get, list, delete

#### Write Data to KV Store:
`vault kv put [secret-path] key=value` example: `vault kv put foobar/my-secrets password=abc`

#### Read Data to KV Store:
`vault kv get foobar/my-secrets`

#### List secret path in KV Store:
`vault kv list foobar`
keep adding the subpath to the superpath as you are using the above cmd until you get the complete full path

#### Delete Data in KV Store:
`vault kv delete foobar/my-secrets`

#### Disable KV Store:
`vault secrets disable foobar`