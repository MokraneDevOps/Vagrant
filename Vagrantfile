# Définition du script à exécuter via le provisionneur shell
$script = <<-SCRIPT
  sudo apt-get update
  sudo apt-get install -y nginx
  sudo systemctl start nginx
  sudo systemctl enable nginx
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provision :hosts do |provisioner|
    # Ajouter un seul nom d'hôte
    provisioner.add_host '10.0.2.2', ['myhost.vagrantup.internal']
  end
  
  # Spécifier la box à utiliser
  config.vm.box = "ubuntu/jammy64"
  
  # Définir le nom d'hôte de la machine virtuelle
  config.vm.hostname = "hosttest"
  
  # Configuration du port redirigé (forwarded port)
  config.vm.network "forwarded_port", guest: 80, host: 8080
  
  # Configuration du réseau privé avec une adresse IP statique
  config.vm.network "private_network", ip: "192.168.58.254"
  
  # Configuration du réseau public avec une adresse IP obtenue via DHCP
  config.vm.network "public_network", type: "dhcp"

  # Configuration du dossier synchronisé (synced folder)
  # config.vm.synced_folder "partage", "/partage"

  # Configuration de la provision pour copier un fichier
  config.vm.provision "file", source: "hosts.txt", destination: "$HOME/"

  # Configuration de la provision shell
  config.vm.provision "shell", inline: $script

  # Configuration de la provision Ansible local
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml.txt"
    ansible.install_mode = "pip"
    ansible.pip_install_cmd = "sudo apt-get install python3-pip -y"
    ansible.version = "2.9.6"
  end

  # Configuration du fournisseur VirtualBox
  config.vm.provider "virtualbox" do |vbox|
    # Configuration de la mémoire et du nombre de CPU
    vbox.memory = 2024
    vbox.cpus = 2
  end
end
