

# server:
	sudo apt update
	sudo apt install openssh-server

sudo nano /etc/ssh/sshd_config

PubkeyAuthentication yes
ChallengeResponseAuthentication no
PasswordAuthentication yes

sudo service ssh restart

## client(macOS):

 ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa
			ssh-copy-id -p 22 user@server_ip
			OR 
			cat ~/.ssh/id_rsa.pub | ssh -p 22 user@server_ip 'cat >> ~/.ssh/authorized_keys'

	ssh 연결 


