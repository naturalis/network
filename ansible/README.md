# Naturalis campus network

# Ansible
- Clone this repo (Ansible folder) and install the roles:
```bash
ansible-galaxy install -r roles/requirements.yml --roles-path ./roles/
```
- Test connections, on hardware:
```bash
ansible network -m ping -i environments/prod
```
Or on vx:
```bash
ansible network -m ping -i environments/staging
```
- On timeouts or errors there is probably a problem with the ssh keys. If on vx, fix manually by connecting as the vagrant user.
- Run the provision playbook, on hardware:
```bash
ansible-playbook playbooks/cumulus_provision.yml -i environments/prod
```
Or on vx:
```bash
ansible-playbook playbooks/cumulus_provision.yml -i environments/staging
```
- There is also an interfaces playbook, which only checks the interfaces for changes. On hardware:
```bash
ansible-playbook playbooks/cumulus_interfaces.yml -i environments/prod
```
Or on vx:
```bash
ansible-playbook playbooks/cumulus_interfaces.yml -i environments/staging
```

# Files
```bash
├── environments/                             # Parent directory for environment-specific directories.
│   │
│   ├── prod/                                 # Contains all files specific to the production environment.
│   │   ├── group_vars/                       # prod specific group_vars files.
│   │   │   └── ...                           # Multiple .yml files for different functions
│   │   ├── host_vars/
│   │   │   └── ...                           # each host has a .yml file with interface definitions
│   │   └── hosts                             # Contains only the hosts in the prod environment.
│   │
│   └── staging/                              # Contains all files specific to the staging (cumulus vx) environment.
│       ├── group_vars/                       # staging specific group_vars files.
│       │   └── ...                           # Multiple .yml files for different functions
│       └── hosts                             # Contains only the hosts in the staging environment.
│  
├── playbooks/
│   ├── cumulus_check.yml                     # Playbook to check and ouput ptm and lldp information.
│   ├── cumulus_interfaces.yml                # Playbook to setup interfaces on spine and leaf switches.
│   ├── cumulus_license.yml                   # Playbook to setup or change license key (initial setup is done by ZTP).
│   ├── cumulus_provision.yml                 # Playbook to do the first provisioning.
│   ├── cumulus_support_make.yml              # Playbook to make and retrieve support files for troubleshooting.
│   ├── cumulus_support_remove.yml            # Playbook to remove support files from switches.
│   ├── cumulus_users.yml                     # Playbook to setup users and ssh keys.
│   ├── opnsense_api.yml                      # Playbook to test API on OPNSense firewall.
│   └── opnsense_config.yml                   # Playbook to set config on OPNSense firewall.
│
├── roles/
│   ├── ansible-cumulus-common           # Will set common settings, like NTP, Hostname, Timezone, MOTD, hostfile
│   ├── ansible-cumulus-interfaces       # Will set interfaces and OSPF routing on spine switches and interfaces and PoE on leaf switches
│   ├── ansible-cumulus-license          # Can change the license key on the switches (initial setup is done by ZTP).
│   ├── ansible-cumulus-metrics          # Installs prometheus node_exporter, configures remote syslog and snmp daemon.
│   ├── ansible-cumulus-ptm              # Sets up the Prescriptive Topology Manager with a correct .dot file.
│   ├── ansible-cumulus-repo             # Adds repo's that are mirrored on the oob mgmt server.
│   ├── ansible-cumulus-users            # Sets up Infra user and removes credentials from cumulus user.
│   ├── ansible-opnsense-api             # Test API on OPNSense firewall.
│   ├── ansible-opnsense-config          # Setup config on OPNSense firewall.
│   └── requirements.yml                 # File to install these roles with ansible-galaxy.
│
│
├── .gitignore                                # File to keep some stuff secret ;) .
├── ansible.cfg                               # default arguments for ansible.
├── linter.sh                                 # Passes all .yml files through yamllint ($pip install yamllint).
└── README.md                                 # This file.

```
