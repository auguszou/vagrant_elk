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
  config.vm.box = "alpine/alpine64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 9200, host: 9200
  config.vm.network "forwarded_port", guest: 9002, host: 9006
  config.vm.network "forwarded_port", guest: 5601, host: 55601

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/vagrant"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end

  config.vbguest.auto_update = false

  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apk update
    apk upgrade
    apk add redis curl wget nginx bash bash-completion ca-certificates openjdk8
		wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub
		wget -q -O /tmp/glibc-2.25-r0.apk https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.25-r0/glibc-2.25-r0.apk
		apk add /tmp/glibc-2.25-r0.apk

    sed -i "s#/bin/ash#/bin/bash#g" /etc/passwd

    cat > /home/vagrant/.bash_profile <<EOF
[[ -f ~/.bashrc ]] && . ~/.bashrc
EOF

    cat > /home/vagrant/.bashrc <<EOF
export HISTTIMEFORMAT="%d/%m/%y %T "
export PS1="\\u@\\h:\\W \\$ "
alias l='ls -CF'
alias la='ls -A'
alias ll='ls -alF'
alias ls='ls --color=auto'

# export JAVA_HOME=/home/vagrant/jdk1.8.0_152
# export PATH=$PATH:$JAVA_HOME/bin
source /etc/profile.d/bash_completion.sh
EOF

	chown -R vagrant:vagrant /home/vagrant/

	# tar xf /vagrant/jdk-8u152-ea-bin-b05-linux-x64-20_jun_2017.tar.gz -C /home/vagrant/
	tar xf /vagrant/elasticsearch-5.6.1.tgz -C /home/vagrant/
	tar xf /vagrant/logstash-5.6.1.tgz -C /home/vagrant/
	tar xf /vagrant/kibana-5.6.1-linux-x86_64.tgz -C /home/vagrant/

	cp -rf /vagrant/nginx.conf /etc/nginx/
	sudo /etc/init.d/nginx restart
	sudo /etc/init.d/redis restart

	su - vagrant -c "nohup /home/vagrant/elasticsearch-5.6.1/bin/elasticsearch 2>&1 >> /tmp/elastic_search.log &"
	su - vagrant -c "nohup /home/vagrant/logstash-5.6.1/bin/logstash -f /home/vagrant/logstash-5.6.1/config/logstash_agent_conf 2>&1 >> /tmp/logstash.log &"
	su - vagrant -c "nohup /home/vagrant/kibana-5.6.1-linux-x86_64/bin/kibana 2>&1 >> /tmp/kibana.log &"
	sleep 1
  SHELL
end