# mattermost-ansible
This is an Ansible Playbook that installs a standalone version of Mattermost, which is an open-source Slack alternative.
This playbook installs Mattermost version 5.27.0 (Team Edition) by default.

It downloads the binary from [mattermost.org](https://www.mattermost.org/download/). If you need to install the Enterprise
edition, consult the Mattermost documentation. This playbook installs
 * Database Server `postgresql` or `mariadb` (with galera cluster)
 * Web Server (acts as a reverse proxy) - `nginx`
 * SSL certificates are automatically generated from the [letsencrypt](https://letsencrypt.org) project. A cron job is
 created that automatically renews the SSL certificates once a month.

---


This playbook currently works with
- [x] Ubuntu 16.04 LTS
- [x] Ubuntu 18.04 LTS
- [x] Ubuntu 20.04 LTS
- [x] CentOS 7.3 (DigitalOcean)
- [x] Red Hat Enterprise Linux 7.3 (Maipo) (Installed from RedHat DVD on a Vultr VPS)
- [x] Debian 8.8 Jessie (thanks [fgbreel](https://github.com/fgbreel)!)


---

## Usage
* Install ansible with your package manager of choice. Ansible can also be installed via `pip`. This playbook was tested with Ansible 2.9.9 If you can, I would recommend running the most recent version of ansible.


* Clone this repository.

* Make sure that the server you are installing Mattermost is properly configured with a FQDN. You should also have root
 access via ssh. Reverse DNS should also be properly configured for letsencrypt to work. If you are using a cloud
 hosting provider, this should be trivial.

* Edit `play.yml` and change the `vars` to reflect any changes you may want to make for your system. This playbook does
not do a complete installation with full configuration of all of the Mattermost options, but rather installs it to the
point where you can edit the relevant settings from within the web browser.

* **You should *always* edit the email address and db_password fields.**

* If you want to deploy mariadb instead of postgres, you should set `mattermost_postgres` to false
In this case you will also need to install requirements with `ansible-galaxy install -r requirements.yaml`

* Adjust `inventory` file in the project directory. It only needs to contain one line, which is the IP address of the server
you wish to install Mattermost on. If you wish to install galera cluster, `galera_all` group should exist.

* Run `ansible-playbook play.yml -i inventory` from the top of level of the project directory.

* Navigate to the FQDN of your server in a web browser. Consult the Mattermost documentation for further configuration
options.

---

## Post-Install
If you are planning to use MatterMost for any length of time, you should probably change the location of the
data directory. A large volume of attached block storage would not be a bad idea. A working email server should also
be configured for email notifications and invites.  You can do most of this from within the browser without manually editing
configuration files.

---

### Contributing
Please submit pull requests! They make my day.
