Chef Solo demo with Vagrant
===========================

This Vagrant machine shows how to install Chef SDK and run chef-solo. Usage:

```
vagrant up
```

It clones a simple cookbook which creates a file (index.html) in /root. ack cookbook is a dependency and it is installed in /var/chef/cookbooks with Berkshelf. If you do not want to use Berkshelf, you need to copy all cookbooks in /var/chef/cookbooks, and remove berks command from the provisioning script. The provisioning script is in Vagrantfile.
