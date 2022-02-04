# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ahplummer/PopOS_21.10"
  config.vm.guest = "ubuntu"

    # Password Input Function
  class Password
    def to_s
      begin
      system 'stty -echo'
      print "Ansible Vault Password: "
      pass = URI.escape(STDIN.gets.chomp)
      ensure
      system 'stty echo'
      end
      print "\n"
      pass
    end
  end

    # Ask for vault password
  config.vm.provision "shell", env: {"VAULT_PASS" => Password.new}, inline: <<-SHELL
    echo "$VAULT_PASS" > /tmp/vault_pass
  SHELL

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "main.yml"
    ansible.galaxy_role_file = 'requirements.yml'
    ansible.vault_password_file = "/tmp/vault_pass"
  end
  config.ssh.forward_agent = true

  # Delete temp vault password file
  config.vm.provision "shell", inline: <<-SHELL
    rm /tmp/vault_pass
  SHELL

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = true

end
