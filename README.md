# 🔐 Ansible Collection - `application_infra`

This Ansible collection provides reusable roles for automating Red Hat Build of Keycloak (RHBK) configuration. It includes tasks to programmatically create realms and clients using declarative variables—ideal for secure, repeatable identity management setup in OpenShift and enterprise environments.

The collection simplifies Keycloak bootstrapping and supports GitOps-friendly integration workflows.

---

## ✅ Collection Requirements

- Ansible 2.14+ or AAP 2.4+
- Keycloak (Red Hat Build) deployed and accessible
- Installed collections:
  - `community.kubernetes`
  - `kubernetes.core`
- Cluster credentials or service account permissions to interact with Keycloak

---

## 📦 Included Roles

| Role Name             | Purpose                                                           |
|----------------------|-------------------------------------------------------------------|
| `rhbk_create_realm`  | Creates a Keycloak realm with optional display name and themes    |
| `rhbk_create_client` | Registers a Keycloak client with protocol mappers and settings    |

---

## 🔧 Global Variables Example (vault.yaml or host vars)

| Variable                     | Description                                        | Required |
|-----------------------------|----------------------------------------------------|----------|
| `keycloak_hostname`         | FQDN of Keycloak instance                         | ✅       |
| `keycloak_admin_user`       | Admin user to authenticate against Keycloak       | ✅       |
| `keycloak_admin_password`   | Admin password                                     | ✅       |
| `realm_id`                  | ID of the realm to create                         | ✅       |
| `client_id`                 | ID of the client to create                        | ✅       |
| `client_protocol`           | Protocol (`openid-connect`, `saml`, etc.)         | ✅       |

---

## 🚀 Example: Create Realm and Client in Keycloak

```yaml
- name: Bootstrap RHBK Realm and Client
  hosts: localhost
  gather_facts: false
  collections:
    - fbi.application_infra

  vars_files:
    - vault.yaml

  vars:
    keycloak_hostname: keycloak.apps.ocp.server.lab
    keycloak_admin_user: admin
    keycloak_admin_password: redhat123

    realm_id: "my-app-realm"
    client_id: "grafana-client"
    client_protocol: "openid-connect"

  roles:
    - role: fbi.application_infra.rhbk_create_realm
    - role: fbi.application_infra.rhbk_create_client
```

**Structure**
```
application_infra/
├── docs/
├── galaxy.yml
├── meta/
│   └── runtime.yml
├── plugins/
│   └── README.md
├── README.md
└── roles/
    ├── rhbk_create_client/
    │   ├── defaults/
    │   ├── files/
    │   ├── handlers/
    │   ├── meta/
    │   ├── tasks/
    │   ├── templates/
    │   ├── tests/
    │   └── vars/
    └── rhbk_create_realm/
        ├── defaults/
        ├── files/
        ├── handlers/
        ├── meta/
        ├── tasks/
        ├── templates/
        ├── tests/
        └── vars/

