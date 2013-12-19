# Vagrant-Cassandra

A Vagrant/Berkshelf setup to create a Cassandra cluster. Useful for local testing and experimentation with Cassandra. 

## Dependencies

* Ruby 1.8.7+
* Rubygems
* Vagrant
* Berkshelf
* vagrant-berkshelf Vagrant plugin
* vagrant-omnibus Vagrant plugin

Here are some install commands, if you need them:

    vagrant plugin install vagrant-berkshelf

    vagrant plugin install vagrant-omnibus

## Usage

With all plugins installed, you just need to issue the following command:

    vagrant up

Vagrant will get all of the necessary resources, including base boxes and chef recipes, and create the VMs in your Cassandra cluster. The default number of VMs is 3, but if you want to change that, just edit the Vagrantfile and change the following line:

    server_count = 3

