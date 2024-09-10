Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.define "vm1" do |vm|
    vm.vm.network "private_network", ip: "192.168.56.10"

    vm.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get upgrade -y
    SHELL
  end
end
