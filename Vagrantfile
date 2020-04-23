# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
	config.vm.box = "ubuntu/bionic64"
	config.vm.box_version = "20200416.0.0 "
	config.vm.network "forwarded_port", guest: 80, host: 8888, id: "apache"
	config.vm.network "forwarded_port", guest: 9200, host: 9201, id: "elasticsearch"
	config.vm.synced_folder "shared/", "/home/vagrant/shared", create: true
	config.vm.provider "virtualbox" do |v|
	        v.name = "msc-gods-dev-vm"
		v.memory = 2048
		v.cpus = 2
	end
  
 
config.vm.provision "shell", inline: <<-SHELL
	echo "*** adding ubuntugis ppa and updating package lists ***"
	add-apt-repository ppa:ubuntugis/ubuntugis-unstable
	apt-get update
	echo "*** installing git ***"
	apt-get install -y git
	echo "*** installing vim tools ***"
	apt-get install -y vim vim-common vim-runtime vim-tiny
	echo "*** installing gdal ***"
	sudo apt-get install -y gdal-bin
	apt-get install -y libgdal-dev
	export CPLUS_INCLUDE_PATH=/usr/include/gdal
	export C_INCLUDE_PATH=/usr/include/gdal
	echo "*** installing python3-pip and pylint3 ***"
	apt-get install -y python3-pip
	apt-get install -y pylint3
	echo " *** install GDAL python bindings ***"
	pip3 install GDAL
	echo "*** installing mapserver ***"
	apt-get install -y apache2 apache2-bin apache2-utils cgi-mapserver mapserver-bin mapserver-doc python-mapscript
	a2enmod cgi fcgid
	echo "*** setting Apache default configuration for mapserver ***"
	cat >> /etc/apache2/sites-available/000-default.conf <<EOF
	ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
		<Directory "/usr/lib/cgi-bin/">
		AllowOverride All
			Options +ExecCGI -MultiViews +FollowSymLinks
			AddHandler fcgid-script .fcgi
			Require all granted
		</Directory>
EOF
	service apache2 restart
	echo "*** Mapserver now available from host computer at http://localhost:8888/cgi-bin/mapserv?SERVICE=WMS&VERSION=1.1.1&REQUEST=GetCapabilities ***"
	echo "*** installing and setting up elasticsearch ***"
	wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
	apt-get install apt-transport-https
	echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
	apt-get update && apt-get install elasticsearch
	sudo /bin/systemctl daemon-reload
	sudo /bin/systemctl enable elasticsearch.service
	sudo echo "network.host: 0.0.0.0" >> /etc/elasticsearch/elasticsearch.yml
	sudo echo "http.port: 9200" >> /etc/elasticsearch/elasticsearch.yml
	sudo echo "discovery.type: single-node" >> /etc/elasticsearch/elasticsearch.yml
	sudo systemctl start elasticsearch.service
	echo "*** Elasticsearch now available on host at http://localhost:9201 ****"
	SHELL
end
