
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.synced_folder "./configs", "/configs"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Criando a maquina ansible
  config.vm.define "ansible" do |ansible|
    ansible.vm.network "public_network", ip: "192.168.15.20"
    ansible.vm.provider "virtualbox" do |vb|
      vb.name = "ansible"
      vb.memory = 512
    end

    # Atualizando o sistema e instalando o Ansible
    ansible.vm.provision "shell",
      inline: "apt update && \
               apt install -y software-properties-common && \
               apt-add-repository --yes --update ppa:ansible/ansible && \
               apt install -y ansible"

    # Copiando chave privada para dentro da maquina ansible
    ansible.vm.provision "shell",
      inline: "cp /configs/id_bionic /home/vagrant && \
               chmod 600 /home/vagrant/id_bionic && \
               chown vagrant:vagrant /home/vagrant/id_bionic"

  end

  # Criando a maquina wordpress
  config.vm.define "wordpress" do |wordpress|

    wordpress.vm.box = "bento/ubuntu-20.04"
    wordpress.vm.network "public_network", ip: "192.168.15.21"

    wordpress.vm.provider "virtualbox" do |vb|
      vb.name = "wordpress"
    end

    # Copiando chave publica para dentro da maquina php5
    wordpress.vm.provision "shell",
      inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"

    # Atualizando sistema operacional
    wordpress.vm.provision "shell",
      inline: "apt-get update"

  end

end
