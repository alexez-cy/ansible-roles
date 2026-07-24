This Ansible project deploys an Elasticsearch 9.x cluster secured with ReadonlyREST and installs Kibana on Ubuntu hosts.

The deployment is intended to be executed from AWX or Ansible using an inventory group representing a single Elasticsearch cluster.

## Features

- Installs Elasticsearch 9.x
- Installs Kibana 9.x
- Downloads ReadonlyREST directly from the vendor portal
- Installs and patches ReadonlyREST for Elasticsearch and Kibana
- Configures cluster discovery automatically
- Determines Elasticsearch node roles from hostnames
- Retrieves TLS certificates and credentials from HashiCorp Vault
- Supports idempotent reinstallation of ReadonlyREST

## Requirements

- Ubuntu
- Ansible / AWX
- HashiCorp Vault
- ReadonlyREST subscription
- Internet access to the ReadonlyREST download portal

## Inventory

The inventory group name becomes the Elasticsearch cluster name.

## Node Roles

Node roles are assigned automatically from the hostname.

| Hostname contains | Elasticsearch role |
|-------------------|--------------------|
| `master` | `master` |
| `data` | `data` |
| `ingest` | `ingest` |
| none | `master,data` |

Examples:

| Hostname | Roles |
|----------|-------|
| `elk-master1` | `master` |
| `elk-data1` | `data` |
| `elk-ingest1` | `ingest` |
| `elk-node1` | `master,data` |

## Variables

| Variable | Description |
|----------|-------------|
| `es_version` | Elasticsearch version (e.g. `9.4.1`) |
| `ror_version` | ReadonlyREST version (e.g. `1.69.1`) |
| `ror_email` | Email used to download ReadonlyREST |
| `es_data_path` | Elasticsearch data directory |
| `domain` | Domain to be used in Kibana server.publicBaseUrl |

## Kibana

Kibana is installed on every node **except** hosts whose hostname contains `data`.

## TLS

During deployment the playbook retrieves the following secrets from HashiCorp Vault:

- TLS certificate
- Private key
- ReadonlyREST admin password
- ReadonlyREST Kibana password

The passwords are converted to SHA-256 hashes before generating the ReadonlyREST configuration.

## Playbook

The deployment playbook:

- retrieves secrets from HashiCorp Vault
- prepares certificates and password hashes
- deploys Elasticsearch to every selected host
- deploys Kibana only to non-data nodes

## Running

Deploy a cluster by limiting execution to the inventory group:

```bash
ansible-playbook playbook.yml --limit
```

or from AWX by selecting the required inventory and setting the limit to the target cluster group.

## Notes

- The inventory group name is used as the Elasticsearch cluster name.
- `discovery.seed_hosts` is generated using only master-eligible nodes.
- `cluster.initial_master_nodes` is used only during the initial cluster bootstrap.
- ReadonlyREST is automatically unpatched, removed, reinstalled and patched during upgrades.
- The deployment is fully idempotent and can be safely re-run.
