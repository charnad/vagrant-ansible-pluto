VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  
  config.vm.define "pluto" do |pluto|
    pluto.vm.hostname = "pluto"

    # VirtualBox settings
    pluto.vm.provider "virtualbox" do |v|
      v.gui = true
      v.name = "Pluto"
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.cpus = 2
      v.memory = 512
      #v.ioapic = 1
    end
    
    # Network
    pluto.vm.network :private_network, ip: "192.168.100.9"
    pluto.vm.network :public_network

    # Provision
    pluto.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/playbook.yml"
        ansible.host_key_checking = "false"
    end

    # Triggers
    pluto.trigger.after  :up,      :run => "mount_smbfs cifs://vagrant:vagrant@192.168.100.9/development development"
    pluto.trigger.before :destroy, :run => "umount development"
    pluto.trigger.before :halt,    :run => "umount development"
  end
end