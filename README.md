# Homelab

## Repository architecture

```
homelab/
├── Makefile
├── README.md
├── .env.example                  # Expected environment variables
├── .gitignore
├── .gitattributes                # Enforces LF line endings
│
├── infrastructure/
│   ├── ansible/
│   │   ├── ansible.cfg
│   │   ├── requirements.yml      # Collections Ansible (community.general, ...)
│   │   ├── inventories/
│   │   │   └── production/
│   │   │       ├── hosts.yml
│   │   │       └── group_vars/
│   │   │           └── all.yml
│   │   ├── playbooks/
│   │   │   ├── site.yml             # Runs the 3 playbooks below in sequence
│   │   │   ├── provision-vms.yml    # Creates VMs on Proxmox
│   │   │   ├── configure-vms.yml    # System and Docker configuration
│   │   │   └── deploy-services.yml  # Deploys the apps
│   │   └── roles/
│   │       ├── proxmox_vm/
│   │       ├── common/
│   │       ├── docker/
│   │       ├── securo/
│   │       ├── cloudflared/
│   │       └── pihole/
│   │
│   └── apps/                     # One deployed app = one directory
│       ├── securo/
│       │   ├── compose.yml
│       │   ├── compose.override.yml
│       │   └── .env.j2
│       ├── cloudflared/
│       │   └── compose.yml
│       └── pihole/
│           ├── compose.yml
│           └── .env.j2
│
├── recovery/
│   └── README.md
│
└── baremetal-provisioning/       # Later
    └── README.md
```

## Secrets

Secrets are never committed: they are injected through environment variables
and read by Ansible (`lookup('ansible.builtin.env', ...)`).

- **Local (WSL)**: copy `.env.example` to `.env` and fill it in.
- **CI (GitHub Actions, later)**: define secrets in the repository using the same names.
