# Ubuntu Server in Hper-V and MiniKube
## Install
'''
	Net (ip fija GW y DNS a la maquina anfritiona)
		Net: 192.168.2.0/24
		Ip:192.168.2.2
		GW:192.168.2.1
		DNS:192.168.2.1
		Domain: local
	Packetes: Ninguno
'''

## First Steps
'''
	sudo lvdisplay
	sudo lvextend -L +8G /dev/ubuntu-vg/ubuntu-lv
	sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
	sudo apt-get update
	sudo apt-get upgrade -y
	sudo apt-get dist-upgrade -y
	sudo apt autoremove -y
	sudo reboot
	sudo nano /etc/initramfs-tools/modules
'''
Add the next lines
'''
hv_vmbus
hv_storvsc
hv_blkvsc
hv_netvsc
'''
Finish
'''
	sudo apt-get install linux-virtual linux-cloud-tools-virtual linux-tools-virtual
	sudo update-initramfs -u
	sudo reboot
	lsmod (buscar hv_vmvus)
'''	
## Docker
'''
	sudo apt-get remove docker docker-engine docker.io
	sudo apt-get install apt-transport-https ca-certificates curl software-properties-common -y
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu disco stable"
	sudo apt-get update
	sudo apt-get install docker-ce
	sudo groupadd docker
	sudo usermod -a -G docker daniel
'''

## Kubectl
'''
	sudo apt-get update && sudo apt-get install -y apt-transport-https
	curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
	echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
	sudo apt-get update
	sudo apt-get install -y kubectl
'''

## Minikube
'''
	curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
	sudo mkdir -p /usr/local/bin/
	sudo install minikube /usr/local/bin/
	sudo minikube start --vm-driver=none
'''
