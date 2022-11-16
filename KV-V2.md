When running v2 of the kv backend, a key can retain a configurable number of versions. This defaults to 10 versions.

#### Enable KV V2 Secret Engine:
`vault secrets enable kv-v2` cmd will mount the kv store in the default i.e kv

#### Enable KV V2 Secret Engine with a custom path:
`vault secrets enable -path=foobar-v2 kv-v2`

#### KV V2 sub-commands:
delete, destroy, enable-versioning, get, list, metadata, patch, put, rollback, undelete

#### Write Data to KV V2 Store:
`vault kv put [secret-path] key=value` 

**example**: `vault kv put foobar-v2/my-secrets password=abc`

#### Enable Versioning, upgrade Version KV V1 to V2:
`vault kv enable-versioning foobar`

#### Write & Read from Upgraded KV Store
when you write to the path of the upgraded kv store, it will store the data as version 2, and to read the previous data stored, use 
**"-version=1"**: `vault kv get -version=1 foobar/my-secrets`

#### Delete & Undelete Data in KV Store:
The latest version of a key can be deleted with the delete command. It also takes a **-versions** flag to delete prior versions. 

When you delete data that is versioned, it does a soft delete. It puts a delete marker in there and does not delete the metadata. The "secret-path could be a v1 or v2
`vault kv delete -versions=# [secret-path]`
`vault kv undelete -versions=# [secret-path]`

#### List secret path in KV Store:
`vault kv list foobar-v2`
keep adding the subpath to the superpath as you are using the above cmd until you get the complete full path

#### View Metadata of your Secret:
`vault kv metadata get [secret-path]`

#### Set maximum version for your kv store:
`vault kv metadata put -max-versions # [secret-path]`

If you have 4 versions of your data prior to setting the maximum version as 2, after setting the max version, it will delete the oldest 2 versions and keep the recent 2 since the max version has been set to 2

#### permanently Remove the Specified Versions' data:
`vault kv destroy -version=# [secret-path]`

#### Permanently Delete all Versioned Secrets and Metadata
`vault kv metadata delete [secret-path]`

#### Disable KV Store:
`vault secrets disable [secret-path]`
