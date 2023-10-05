Vagrant.configure("2") do |config|
  # Maquina 01 (clienteServidor)
  config.vm.define "cliente" do |cliente|
    cliente.vm.box = "ubuntu/trusty64"
    cliente.vm.network "private_network", type: "static", ip: "192.168.0.3"
    cliente.vm.hostname = "JuanReboucasRamonBento"
    cliente.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y curl vim htop
    SHELL
  end

   # Maquina 02 (Servidor2)
   config.vm.define "servidor" do |servidor|
    servidor.vm.box = "centos/7"
    servidor.vm.network "private_network", type: "static", ip: "192.168.0.4"
    servidor.vm.hostname = "JuanReboucasRamonBento"
    servidor.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
    servidor.vm.provision "shell", inline: <<-SHELL
      sudo yum install -y epel-release
      sudo yum install -y docker git wget
      sudo yum install -y docker-compose
      sudo systemctl start docker
      sudo systemctl enable docker
    SHELL
  end
end