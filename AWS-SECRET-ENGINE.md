Dynamically generates aws credentials to access our aws accounts based on iam policies. When you generate credentials for aws, the credentials are still bound by your traditional iam policies which would deny or permit access to actions and resources within aws. It is simple to use for basic usage but trickier when using multiple accounts.

### Steps:
- **enable aws secrets engine:** 
By default, the secrets engine will mount at the name of the engine,`vault secrets enable aws`.
To enable the secrets engine at a different path, use the `-path` argument, `vault secrets enable -path=foobar/aws-secrets aws`

- **configure the credentials that vault uses to communicate with AWS to generate the IAM credentials:** 
```
vault write aws/config/root \
    access_key=..... \
    secret_key=...... \
    region=...."
```    
*Even though the path above is aws/config/root, do not use your AWS root account   credentials. Instead generate a dedicated user or role*

- **Configure a Vault role that maps to a set of permissions in AWS as well as an AWS   credential type. When users generate credentials, they are generated against this role:**
```
vault write aws/roles/my-role \
    credential_type=iam_user \
    policy_document=-<<EOF
    {
        "Version": "2012-10-17",
        "Statement": [
            {
              "Effect": "Allow",
              "Action": "ec2:*",
              "Resource": "*"
            }
        ]
    }
EOF
```
This creates a role named "my-role" (could be any name). When users generate credentials against this role, Vault will create an IAM user and attach the specified policy document to the IAM user. Vault will then create an access key and secret key for the IAM user and return these credentials. you can add a ttl after my-role (.../my-role ttl=60s)

- **Generate a new credential by reading from the /creds endpoint with the name of the role:**
`vault read aws/creds/my-role`
*Each invocation of the command will generate a new credential*