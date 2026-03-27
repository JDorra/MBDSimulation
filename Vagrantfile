Vagrant.configure("2") do |config|
    config.vm.box = "debian/contrib-buster64"  
    # Fix for Debian 10 EOL: redirect to archived repositories and ignore expired metadata
    config.vm.provision "shell", inline: <<-SHELL
      sed -i 's|http://deb.debian.org/debian|http://archive.debian.org/debian|g' /etc/apt/sources.list
      sed -i 's|http://security.debian.org/debian-security|http://archive.debian.org/debian-security|g' /etc/apt/sources.list
      sed -i '/buster-updates/d' /etc/apt/sources.list
      echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99no-check-valid-until
      apt-get update
    SHELL
    config.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible/vagrant.yml"
    end

    config.vm.provider :virtualbox do |vb|
        # distinguish VMs by a location-dependent suffix
        name_suffix = Digest::SHA1.hexdigest(Dir.pwd)[0..6]

        vb.gui = true
        vb.memory = 2048
        vb.name = "Artery Vagrant VM " + name_suffix
        vb.customize ["modifyvm", :id, "--vram", "32"]
    end
end
