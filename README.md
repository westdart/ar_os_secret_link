# ar_os_secret_link

Link a secret to a Service Account

## Requirements
- oc command line is available
- Execution is made by user able to create secret links (oc secret link) 


## Role Variables
The following details:
- the parameters that should be passed to the role (aka vars)
- the defaults that are held
- the secrets that should generally be sourced from an ansible vault.

### Parameters:

| Variable                    | Description                                  | Default |
| --------                    | -----------                                  | ------- |
| ar_os_secret_link_namespace | Namespace on which to operate                | None    |
| ar_os_secret_link_items     | Object defining the Secret links (See below) | None    |


The structure of the 'ar_os_secret_link_items' is:
```
[
  {
    sa_name: '<the service account name>'
    link_secrets: [
      {
        secret_name: '<the secret name>'
        operation: '<Type of operation ['pull' | 'push']>'
      }, ... 
    ]     
  }, 
  ... 
]
```

### Defaults
| Variable                     | Description                              | Default                                                                                                                                        |
| --------                     | -----------                              | -------                                                                                                                                        |
| ar_os_secret_link_assertions | List of assertions made before execution | 'ar_os_secret_link_namespace' is provided and for each link item 'sa_name' is provided and for each link item secret 'secret_name' is provided |

### Secrets
The following variables should be provided through an encrypted source:

## Example Playbook

```
- hosts: localhost
  tasks:
    - name: Include Secret Links
      include_role:
        name: ar_os_secret_link
      vars:
        ar_os_secret_link_namespace: "sunshine-desserts"
        ar_os_secret_link_items: [
          {
            sa_name: 'default', 
            link_secrets: [
              { secret_name: 't2-registry-credential' }
            ]
          }
        ]
```