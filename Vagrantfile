# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "bento/debian-9"
  config.vm.hostname = 'checkmk-vagrant'

  config.vm.network "private_network", type: "dhcp"

  config.vm.synced_folder "data", "/vagrant_data"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get upgrade
    wget https://checkmk.com/support/Check_MK-pubkey.gpg | apt-key -add -
    sudo apt-get install -y gdebi-core
    sudo gdebi -n /vagrant_data/check-mk-raw-1.5.0p16_0.stretch_amd64.deb
    sudo omd create site
    sudo omd start site
    echo
    ip=$(ip -o a s eth1 | grep /2 | awk '{print $4}' | cut -d/ -f 1)
    echo "In your browser, go to:"
    echo " - http://${ip}/site/"
    echo
    echo "Default credentials are => omdadmin:omd"
    echo
    echo "Happy monitoring!"
  SHELL
end
