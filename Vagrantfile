
Vagrant.configure("2") do |config|
	config.vm.define "web" do |web|
		web.vm.box = "sbeliakou/centos-7.4-x86_64-minimal"
		web.vm.hostname = "web"
		web.ssh.insert_key = false
		web.vm.network "private_network", ip: "172.22.1.100"
		web.vm.provider "virtualbox" do |v|
			v.memory = "1024"
    			v.name = "web"
		end
		web.vm.provision "shell", inline: <<-SHELL
		yum install -y epel-release wget net-tools java-devel
		yum install -y avahi avahi-tools nss-mdns
		wget https://releases.hashicorp.com/serf/0.8.1/serf_0.8.1_linux_amd64.zip
		yum install -y zip unzip
		unzip serf_0.8.1_linux_amd64.zip
		mv serf /usr/local/bin/serf
		chmod +x /usr/local/bin/serf
		mkdir /etc/serf
		cp /vagrant/serf.service /usr/lib/systemd/system/serf.service
		chown -R vagrant:vagrant /etc/serf
		mv serf /usr/bin/serf
		yum install -y nginx
		cp -f /vagrant/nginx.conf /etc/nginx/nginx.conf
		systemctl daemon-reload
		systemctl enable avahi-daemon
 		systemctl start avahi-daemon
		systemctl enable serf.service
		systemctl start serf.service
		systemctl enable nginx.service
		systemctl start nginx.service
		SHELL
	end
#<<'COMMENT'
	(1..2).each do |i|
		config.vm.define "app-#{i}" do |app|
		app.vm.box = "sbeliakou/centos-7.4-x86_64-minimal"
		app.vm.hostname = "app-#{i}"
		app.ssh.insert_key = false
		app.vm.network "private_network", ip: "172.22.1.#{100 + i}"
		app.vm.provider "virtualbox" do |v|
			v.memory = "2048"
			v.name = "app-#{i}"
		end
		app.vm.provision "shell", inline: <<-SHELL
		yum install -y epel-release wget net-tools java-devel
		yum install -y avahi avahi-tools nss-mdns
		cd /tmp
		wget https://releases.hashicorp.com/serf/0.8.1/serf_0.8.1_linux_amd64.zip
		yum install -y zip unzip
		unzip serf_0.8.1_linux_amd64.zip
		mv serf /usr/local/bin/serf
		chmod +x /usr/local/bin/serf
		mkdir /etc/serf
		cp /vagrant/serf.service /usr/lib/systemd/system/serf.service
		chown -R vagrant:vagrant /etc/serf
		mv serf /usr/bin/serf
		yum install -y tomcat-admin-webapps.noarch tomcat-docs-webapp.noarch tomcat-javadoc.noarch tomcat-systemv.noarch tomcat-webapps.noarch
		systemctl enable avahi-daemon
		systemctl start avahi-daemon
		systemctl enable serf.service
		systemctl start serf.service
		systemctl enable tomcat
		systemctl start tomcat
		SHELL
	end
end
#COMMENT
end

