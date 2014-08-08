# Puppet Labs SSH Keys
Puppet Labs engineers and technical people should add their SSH public keys to
the templates directory of this project. These keys are periodically copied to
our Delivery systems that are not managed by Puppet.

# Usage
If a host is not being managed by Puppet, then SSH access can be granted using
this process.

If the host has a cron that uses `/etc/cron.hourly/` then simply copy the
script into `/etc/cron.hourly/` like this:

```bash
cd /etc/cron.hourly/
curl -O https://raw.github.com/puppetlabs/puppetlabs-sshkeys/master/templates/scripts/manage_root_authorized_keys
chmod +x manage_root_authorized_keys
```

If the cron on the system does not support `cron.hourly`, the following
crontab entry may be used.

```bash
mkdir -p /usr/local/bin
cd /usr/local/bin
curl -O https://raw.github.com/puppetlabs/puppetlabs-sshkeys/master/templates/scripts/manage_root_authorized_keys
chmod +x manage_root_authorized_keys
crontab -e
```

The entry should look like:

```
# min hour dom month dow command
59 * * * * /usr/local/bin/manage_root_authorized_keys
```

# Adding Keys
Ideally, keys should be registered in the [User Account
Registry](https://docs.google.com/a/puppetlabs.com/spreadsheet/viewform?hl=en_US&formkey=dGl5YVFEX3R6a3p2Vm5wMlRkNDZaVWc6MQ#gid=2),
then added to the `templates/ssh/` directory of this repository. Add your
public key as `username.pub`, and then append it to `authorized_keys`. This
might look something like:

```bash
# Be sure to replace SSHKEY_DIR and USERNAME with the correct values!
export SSHKEY_DIR=~/working/puppetlabs-sshkeys/templates/ssh
export USERNAME="username"

cp ~/.ssh/id_rsa.pub ${SSHKEY_DIR}/${USERNAME}.pub
cat ${SSHKEY_DIR}/${USERNAME}.pub >> ${SSHKEY_DIR}/authorized_keys
```

Once your pull request has been merged into the master branch, the keys will
automatically be copied to all of the hosts using this script.

