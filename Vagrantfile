# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.host_name = 'solr'

  # the base box this environment is built off of
  config.vm.box = 'precise'

  # the url from where to fetch the base box if it doesn't exist
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

  # attach network adapters
  
  config.vm.network "private_network", ip: '192.168.3.10'

  # use puppet to provision packages, set a fact to indicate which contrib
  # module to support

  config.vm.define :searchapi, {:primary => true} do |searchapi|
    searchapi.vm.provision :puppet do |puppet|
      puppet.manifests_path = 'puppet/manifests'
      puppet.manifest_file = 'solr.pp'
      puppet.module_path = 'puppet/modules'
      puppet.facter = {'drupalsolrmodule' => 'search_api'}
    end
  end

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    v.customize ["modifyvm", :id, "--cpus", "2"]
    v.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.define :apachesolr do |apachesolr|
    apachesolr.vm.provision :puppet do |puppet|
      puppet.manifests_path = 'puppet/manifests'
      puppet.manifest_file = 'solr.pp'
      puppet.module_path = 'puppet/modules'
      puppet.facter = {'drupalsolrmodule' => 'apachesolr'}
    end
  end
end
