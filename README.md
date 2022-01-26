# vagrant-p01-first-look
Primeiro projeto para conhecer Vagrant

! - Arquivos "ssh_key" e "ssh_key.pub" estão vazios e são apenas ilustrativos. Devem ser substituidos por chaves SSH válidas.

Projeto utilizando Vagrant para aprendizado.

O objetivo principal do projeto é criar máquinas virtuais que se comunicam entre si e apresentar diferentes formas de provisionar essas máquinas.
Para isso, uma aplicação PHP, rodando em uma máquina virtual, é capaz de se comunicar com uma base MySQL configurada em outra máquina virtual. 

Criação de máquinas virtuais utilizando, por padrão, ubuntu/bionic64 como box.
Máquina virtual contendo uma base MySQL e configurada usando shell script como provisioner.
Máquina virtual contendo PHP configurado utilizando Puppet como provisioner.
Máquina virtual contendo outra base MySQL, mas configurada usando Ansible como provisioner.
Máquina virtual contendo Ansible para servir de máquina de controle, configurada usando shell script como provisioner.
Máquina virtual para exemplificar a utilização de uma box diferente.
