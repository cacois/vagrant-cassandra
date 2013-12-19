# -*- mode: ruby -*-
# # vi: set ft=ruby :

# Cassandra Cluster Settings
server_count = 3
network = '10.0.1.'
first_ip = 10
cluster_name = 'Cassandra-Cluster'

# vars
servers = []
seeds = []
cassandra_tokens = []

# Generate server configs
(0..server_count-1).each do |i|
  name = 'Node' + (i + 1).to_s
  ip = network + (first_ip + i).to_s
  seeds << ip
  servers << {'name' => name,
    'ip' => ip,
    'initial_token' => 2**127 / server_count * i
  }
end

# Create the VMs
Vagrant.configure("2") do |config|
  # vagrant-berkshelf, vagrant-omnibus is used.
  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest
  servers.each do |server|
    config.vm.define server['name'] do |s|
      s.vm.box = "precise64"
      s.vm.box_url = "http://files.vagrantup.com/precise64.box"
      s.vm.hostname = server['name']
      s.vm.network :private_network, ip: server['ip']

      s.vm.provider "virtualbox" do |v|
        v.name = cluster_name.gsub(/\s*/, '') + '-' + server['name']
      end

      s.vm.provision :chef_solo do |chef|
        chef.log_level = :debug
        chef.add_recipe "apt"
        chef.add_recipe "java"
        chef.add_recipe "cassandra::tarball"
        chef.json = {
          :cassandra => {
            'cluster_name' => cluster_name,
            'initial_token' => server['initial_token'],
            'seeds' => seeds,
            'listen_address' => server['ip'],
            'rpc_address' => server['ip']
          },
          :java => {
            'install_flavor' => "oracle",
            'jdk_version' => "7",
            'oracle' => {
              'accept_oracle_download_terms' => true
            }
          }
        }
      end
    end
  end
end
