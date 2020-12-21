<p align="center">
<img src="https://github.com/crowdsecurity/crowdsec/raw/master/docs/assets/images/crowdsec_logo.png" alt="CrowdSec" title="CrowdSec" width="400" height="240" style="max-width:100%;">
</p>
<p align="center">
<a href='https://github.com/fallard84/cs-cloud-firewall-bouncer/actions?query=branch%3Amain+workflow%3Abuild'><img src='https://github.com/fallard84/cs-cloud-firewall-bouncer/workflows/build/badge.svg?branch=main' alt='Build Status' /></a>
<a href='https://github.com/fallard84/cs-cloud-firewall-bouncer/actions?query=branch%3Amain+workflow%3Atests'><img src='https://github.com/fallard84/cs-cloud-firewall-bouncer/workflows/tests/badge.svg?branch=main' alt='Tests Status' /></a>
<a href='https://coveralls.io/github/fallard84/cs-cloud-firewall-bouncer?branch=main'><img src='https://coveralls.io/repos/github/fallard84/cs-cloud-firewall-bouncer/badge.svg?branch=main' alt='Coverage Status' /></a>
<a href='https://opensource.org/licenses/MIT'><img src='https://img.shields.io/badge/License-MIT-yellow.svg' alt='License: MIT' /></a>

</p>

<p align="center">
&#x1F4DA; <a href="#installation-as-a-systemd-service">Documentation</a>
&#x1F4A0; <a href="https://hub.crowdsec.net">Hub</a>
&#128172; <a href="https://discourse.crowdsec.net">Discourse </a>
</p>

# cs-cloud-firewall-bouncer

Bouncer for cloud firewalls to use with Crowdsec.

:warning: This is not an official Crowdsec bouncer.

cs-cloud-firewall-bouncer will periodically fetch new and expired/removed decisions from CrowdSec Local API and update cloud firewall rules accordingly.

Supported cloud providers:

- Google Cloud Platform (GCP) :heavy_check_mark:

:information_source: More cloud providers will be added shortly. See [todo](#todo)

## Installation (as a systemd service)

### With installer

First, download the latest [`cs-cloud-firewall-bouncer` release](https://github.com/fallard84/cs-cloud-firewall-bouncer/releases).

```sh
$ tar xzvf cs-cloud-firewall-bouncer.tgz
$ sudo ./install.sh
```

### From source

Run the following commands:

```bash
git clone https://github.com/fallard84/cs-cloud-firewall-bouncer.git
cd cs-cloud-firewall-bouncer/
make release
tar xzvf cs-cloud-firewall-bouncer.tgz
cd cs-cloud-firewall-bouncer-v*/
sudo ./install.sh
```

### Start

If your bouncer run on the same machine as your crowdsec local API, you can start the service directly since the `install.sh` took care of the configuration.

```sh
sudo systemctl start cs-cloud-firewall-bouncer
```

### Upgrade

If you already have `cs-cloud-firewall-bouncer` installed as a service, please download the [latest release](https://github.com/fallard84/cs-cloud-firewall-bouncer/releases) and run the following commands to upgrade it:

```bash
tar xzvf cs-cloud-firewall-bouncer.tgz
cd cs-cloud-firewall-bouncer-v*/
sudo ./upgrade.sh
```

## Using Docker

- TODO

## Configuration

Before starting the `cs-cloud-firewall-bouncer` service, please edit the configuration to add your cloud provider configuration, as well as the crowdsec local API url and key.
The default configuration file is located under : `/etc/crowdsec/cs-cloud-firewall-bouncer/`

```sh
$ vim /etc/crowdsec/cs-cloud-firewall-bouncer/cs-cloud-firewall-bouncer.yaml
```

```yaml
cloud_providers:
  gcp:
    project_id: gcp-project-id # optional if using application default credentials, will override project id of the application default credentials
    network: default # mandatory, this is the VPC network where the firewall rules will be created
rule_name_prefix: crowdsec # mandatory, this is the prefix for the firewall rule names
update_frequency: 10s
daemonize: false
log_mode: stdout
log_dir: log/
log_level: info
api_url: <API_URL> # when install, default is "localhost:8080"
api_key: <API_KEY> # Add your API key generated with `cscli bouncers add --name <bouncer_name>`
```

## Todo

- Add Docker image with instructions
- Add AWS Network Firewall as a provider
- Add Azure as a provider
- Add Google Cloud Armor as a provider
- Add AWS WAF as a provider
