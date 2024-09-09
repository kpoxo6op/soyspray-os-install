Vagrant.configure("2") do |config|
  # Define the base box for Ubuntu
  config.vm.box = "ubuntu/jammy64"
  config.vm.boot_timeout = 600

  # Create an array of VM names and their static IP addresses
  vm_settings = [
    { name: "vm1", ip: "192.168.56.10" },
    { name: "vm2", ip: "192.168.56.11" },
    { name: "vm3", ip: "192.168.56.12" }
  ]

  # Loop to create VMs with static IP addresses
  vm_settings.each_with_index do |setting, index|
    config.vm.define setting[:name] do |vm|
      # Set hostname
      vm.vm.hostname = setting[:name]

      # Configure the VM's network with a static IP
      vm.vm.network "private_network", ip: setting[:ip]

      # Port forwarding to allow SSH access from the host machine
      vm.vm.network "forwarded_port", guest: 22, host: 2222 + index, id: "ssh"

      # VirtualBox provider-specific settings
      vm.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
        vb.name = setting[:name]  # Set the name in VirtualBox GUI
        vb.memory = "512"  # Adjust memory if needed
        vb.cpus = 1        # Adjust CPU count if needed
      end

      # Provision VM with a basic shell script to update and install necessary tools
      vm.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install -y openssh-server
      SHELL
    end
  end
end
