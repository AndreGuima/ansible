
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.synced_folder "./configs", "/configs"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Criando a maquina ansible
  config.vm.define "ansible" do |ansible|
    ansible.vm.network "public_network", ip: "192.168.15.20"
    ansible.vm.provider "virtualbox" do |vb|
      vb.name = "ansible"
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

  # Criando a maquina php5
  config.vm.define "php5" do |php5|
    php5.vm.box = "ubuntu/trusty64"
    php5.vm.network "public_network", ip: "192.168.15.21"
    php5.vm.disk :disk, size: "100GB", primary: true

    php5.vm.provider "virtualbox" do |vb|
      vb.name = "php5"
      vb.memory = 1024
    end

    # Copiando chave publica para dentro da maquina php5
    php5.vm.provision "shell",
      inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
  end

  # Criando a maquina wordpress
  # config.vm.define "wordpress" do |wordpress|
  #   wordpress.vm.network "public_network", ip: "192.168.15.22"
  #   wordpress.vm.provider "virtualbox" do |vb|
  #     vb.name = "wordpress"
  #   end
  #   wordpress.vm.provision "shell",
  #     inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
  # end

end
