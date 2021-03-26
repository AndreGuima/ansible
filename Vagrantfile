
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

    # Copiando a chave privada do wordpress para dentro da maquina ansible
    ansible.vm.provision "shell",
      inline: "cp /configs/id_wordpress /home/vagrant && \
               chmod 600 /home/vagrant/id_wordpress && \
               chown vagrant:vagrant /home/vagrant/id_wordpress"

    # Copiando a chave privada do mysql para dentro da maquina ansible
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

    # Copiando chave publica para dentro da maquina wordpress
    wordpress.vm.provision "shell",
      inline: "cat /configs/id_wordpress.pub >> .ssh/authorized_keys"

    # Atualizando sistema operacional
    wordpress.vm.provision "shell",
      inline: "apt-get update"
  end

  # Criando a maquina mysql 8
  config.vm.define "mysql" do |mysql|
    mysql.vm.box = "bento/ubuntu-20.04"
    mysql.vm.network "public_network", ip: "192.168.15.22"

    mysql.vm.provider "virtualbox" do |vb|
      vb.name = "mysql"
    end

    # Copiando chave publica para dentro da maquina mysql
    mysql.vm.provision "shell",
      inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"

    # Atualizando sistema operacional
    mysql.vm.provision "shell",
      inline: "apt-get update"
  end

end
