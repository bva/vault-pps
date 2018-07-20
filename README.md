# Vault plugin for Pleasant Password Server

Plugin for Hashicorp's Vault, which connect it to Pleasant Password Server.

## Installation

Download and build Go plugin sources:

```
go get -u https://github.com/bva/vault-pps.git
```
You will need to define a plugin directory using the plugin_directory configuration directive in Vault configutration,
then place the vault-pss executable generated above in the directory.

```bash
cp $GOBIN/vault-pps /etc/vault/plugins
export VAULT_PPS_SHA256_SUM=`shasum -a 256 /etc/vault/plugins/vault-pps | awk '{ print $1; }'`
```

Register vault-pps plugin with vault and mount it in Vault _pps_ path:

```bash
vault write sys/plugins/catalog/vault-pps sha_256=$VAULT_PPS_SHA256_SUM command=vault-ppps
vault secrets enable -path=pps -plugin-name=vault-pps plugin
vault write pps/config/access url="$PPS_URL" user_name="$PPS_USER" password="$PPS_PASSWORD"
```

| Variable             | Vault                                                                               |
| ---------------------|-------------------------------------------------------------------------------------|
| PPS_URL              | URL for Pleasant Password Server (https://localhost:8001/)                          |
| PPS_USER             | Username in Pleasant Password Server under which Vault plugin connects              |
| PPS_PASSWORD         | Password for username in Pleasant Password Server under which Vault plugin connects |


Accessing Pleasant Password secrets from Vault:

```bash
vault kv get pps/<folder>/<secret>/fields
vault kv get pps/<folder>/<secret>/custom_fields
vault kv get pps/<folder>/<secret>/attachments
```
