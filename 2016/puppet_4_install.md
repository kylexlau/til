# How to install Puppet server 4

## preinstall

- Master server should be a machine with a fast processor, lots of
  RAM, and a fast disk.
- Agent default to contacting the master at the hostname `puppet`. Add
  it to hosts file or DNS.
- Agent connect to the master on port 8140
- Every node must have a unique hostname.  Forward and reverse DNS
  must both be configured correctly. 
- Master server's time must be accurate because it act as a
  certificate authority.

## install

```bash
rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-6.noarch.rpm
yum install puppetserver
```

## configure
```bash
/opt/puppetlabs/bin/puppet resource service puppetserver ensure=running enable=true 
/opt/puppetlabs/bin/puppet resource service puppet ensure=running enable=true 

# ssl cert
vi /etc/puppetlabs/puppet/puppet.conf
dns_alt_names=puppet,hd-puppet-master,puppetmaster.company.com

# auto sign
vi /etc/puppetlabs/puppet/autosign.conf
*
```
