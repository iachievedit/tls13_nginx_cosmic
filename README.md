A playbook for playing with TLS 1.3!

Our companion article on configuring NGINX to support TLS 1.3 https://dev.iachieved.it/iachievedit/tls-1-3-with-nginx-and-ubuntu-18-10/

Test your browser!  https://tls13.iachieved.it

# Requirements

If you've used Let's Encrypt before with DNS challenges, and happen to have your DNS hosted by Digital Ocean, you're in luck, this playbook should work out of the box with some tweaks to a few Ansible variables.

## Ansible Variables

| Variable | Location | Description |
|----------|----------|-------------|
| `DOMAIN` | `group_vars/all/vars.yml` | Used by the scripts `create_digitalocean_dns_record.sh` and `delete_digitalocean_dns_record.sh` to specify the domain to manipulate DNS records in. See https://developers.digitalocean.com/documentation/v2/#domain-records for details on the Digital Ocean Domain Records API. |
| `DIGITALOCEAN_OAUTH_TOKEN` | `group_vars/all/vars.yml` | Set to the value of `VAULT_DIGITALOCEAN_OAUTH_TOKEN` (stored in `group_vars/all/vault`).  If using the Digital Ocean REST API, create a new vault with the value of your OAuth token. |
| `HOSTNAME` | `host_vars/<hostname>/vars.yml` | Set your webserver host's FQDN, e.g., `tls13.iachieved.it` |
| `LE_EMAIL` | `host_vars/<hostname>/vars.yml` | Set to the e-mail address used for working with Let's Encrypt, e.g., `admin@iachieved.it` |
| `SSL_PROTOCOLS` | `host_vars/<hostname>/vars.yml` | Set to the TLS versions you want your webserver to support.  The string is injected directly into `nginx.conf` for `ssl_protocols`. |
| `SSL_CIPHERS` | `host_vars/<hostname>/vars.yml` | Set to the SSL ciphers you want your webserver to offer.  For TLS1.3, this setting is not used, so set to `''`. |

## Playbook Basics

`playbook.yml` is pretty simple as playbooks go.  

* `certbot` is installed
* `nginx` is installed
* helper scripts for Let's Encrypt DNS validation are installed
* `certbot_renew_certificate.sh` is used in conjunction with the helpers to install a certificate
* `nginx` is (re)started

