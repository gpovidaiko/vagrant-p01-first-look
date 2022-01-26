Vagrant.configure("2") do |config|
	config.vm.box = "ubuntu/bionic64"

	config.vm.provider "virtualbox" do |vb|
		vb.memory = 512
		vb.cpus = 1
	end
	
	config.vm.define "mysqldb" do |mysql|
		mysql.vm.network "forwarded_port",
			guest: 80, host: 8081
		mysql.vm.network "public_network",
			ip: "192.168.0.100"

		mysql.vm.synced_folder ".", "/vagrant", disabled: true
		mysql.vm.synced_folder "./conf", "/conf"

		# Configurando chave ssh pública
		mysql.vm.provision "shell",
			inline: "cat /conf/ssh_key.pub >> .ssh/authorized_keys"
		# Instalando MySQL Server 5.7
		mysql.vm.provision "shell",
			inline: $script_mysql
		mysql.vm.provision "shell",
			inline: "cat /conf/mysqld.cnf > /etc/mysql/mysql.conf.d/mysqld.cnf"
		mysql.vm.provision "shell",
			inline: "service mysql restart"
        mysql.vm.provider "virtualbox" do |vb|
            vb.name = "mysqldb_vagrant"
        end
	end

	config.vm.define "phpweb" do |phpweb|
		phpweb.vm.network "forwarded_port",
			guest: 8888, host: 8888
		phpweb.vm.network "public_network",
			ip: "192.168.0.101"

		# Instalando Puppet
		phpweb.vm.provision "shell",
			inline: $script_puppet
		phpweb.vm.provision "puppet" do |puppet|
			puppet.manifests_path = "./conf/manifests"
			puppet.manifest_file = "phpweb.pp"
		end
        phpweb.vm.provider "virtualbox" do |vb|
            vb.memory = 1024
            vb.cpus = 2
            vb.name = "phpweb_vagrant"
        end
	end
	
	config.vm.define "mysqlserver" do |mysqlserver|
		mysqlserver.vm.network "public_network",
			ip: "192.168.0.200"
		mysqlserver.vm.provision "shell",
			inline: "cat /vagrant/conf/ssh_key.pub >> .ssh/authorized_keys"
        mysqlserver.vm.provider "virtualbox" do |vb|
            vb.name = "mysqlserver_vagrant"
        end
	end
	
	config.vm.define "ansible" do |ansible|
		ansible.vm.network "public_network",
			ip: "192.168.0.201"
		# Instalando Ansible
		ansible.vm.provision "shell",
			inline: $script_ansible
		ansible.vm.provision "shell",
			inline: "cp /vagrant/conf/ssh_key /home/vagrant && \
				chmod 600 /home/vagrant/ssh_key && \
				chown vagrant:vagrant /home/vagrant/ssh_key"
		# Executando Playbook
		ansible.vm.provision "shell",
			inline: "ansible-playbook -i /vagrant/conf/ansible/hosts \
				/vagrant/conf/ansible/playbook.yml"
	end
	
	config.vm.define "memcached" do |memcached|
        memcached.vm.box = "centos/7"
        memcached.vm.provider "virtualbox" do |vb|
            vb.name = "centos7_memcached"
        end
	end
end

$script_mysql = <<-'SCRIPT'
	apt-get update && \
	apt-get install -y mysql-server-5.7 && \
	mysql -e "create user 'user'@'%' identified by 'pass'"
SCRIPT

$script_puppet = <<-'SCRIPT'
	apt-get update && \
	apt-get install -y puppet 
SCRIPT

#	apt-add-repository --yes --update ppa:ansible/ansible && \
$script_ansible = <<-'SCRIPT'
	apt-get update && \
	apt-get install -y software-properties-common && \
	apt-get install -y ansible
SCRIPT