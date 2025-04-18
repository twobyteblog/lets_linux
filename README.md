## Let's Encrypt Certificate Deployment Script for Debian-based Hosts

This roles automates the deployment of Let's Encrypt ```certbot``` utility on Debian-based hosts. The role utilizes the [Cloudflare plugin](https://certbot-dns-cloudflare.readthedocs.io/en/stable/) for Domain Control Validation (DCV).

## Features

- Automates the **certificate renewal** process on a Debian-based host.
- Supports **Debian 11** and above.
- Uses **Cloudflare's API** for Domain Control Validation (DCV).
- Installs certificates into **apache2** and **nginx**.

## Prerequisites

- Debian 11 or above.
- Cloudflare access (to generate API token).

## Actions Performed

The role performs the following actions:

1. Installs ```snapd``` package.
2. Installs ```certbot``` snap.
3. Installs ```certbot-dns-cloudflare``` plugin.
4. Configures logrotate.
5. Stores and saves Cloudflare API token.
6. Generates certificates.
7. Installs certificates for ```apache2``` and ```nginx```.

## Setup Instructions

### Clone Repository

Clone the project into the roles directory of your Ansible Controller.

```bash
git clone https://github.com/twobyteblog/lets_linux.git
```

### Cloudflare API Token

Create a API token to allow ```certbot``` to use CloudFlare for DNS-based Domain Control Validation (DCV). For this to work, the domain your requesting a signed certificate for must be added to your Cloudflare account.

1. Browse to Cloudflare's [User API Tokens](https://dash.cloudflare.com/profile/api-tokens).
2. Create a new Token using the **Edit zone DNS** template. Limit the token to only the domain(s) required.
3. Document the API Token value.

### Variables

Within your Ansible environment, add the following variables:

```bash
# Cloudflare API token created in the previous step.
cloudflare_api_token: "token"

# Location to save Cloudflare token.
cloudflare_api_token_location: "/root/.cloudflare_token"

# Domains. This will generate two separate certificates.
cert_domains:
  - twobyte.blog
  - twobyte.ca
```

### PlayBook

Create a playbook which runs this role. This role requires ```become``` privileges.

```bash
- hosts: all
  roles:
    - role: lets_linux
      become: yes
```




