# Devclub-1
Проверка тестового задания:

В рамках тестового задания я развернул 3 виртуальных машины с помощью системы виртуализации VirtualBox и Vagrant.
Виртуальные машины:
	control - вм под управлением Ubuntu 20.04, на которую будет установлен ansible
	node1 - вм под управлением Ubuntu 20.04, на которую будет установлен nginx
	node2 - вм под управлением Ubuntu 20.04, на которую будет установлен jenkins
	
Во время развертывания виртуальных машин с помощью Vagrant, на виртуальную машину control устанавливается ansible.
Для запуска лабораторного стенда необходимо:
1. Скачать образ ubuntu/focal64 (https://disk.yandex.ru/d/Bw9u36wsOB2PIg)(https://app.vagrantup.com/ubuntu/boxes/focal64/versions/20220419.0.0/providers/virtualbox.box)
2. В командной строке выполнить vagrant box add ubuntu/focal64 "путь к скачанному файлу"
3. В командной строке перейти в директорию с проектом (где лежит файл vagrantfile), например: cd Z:\DevOps\iac.spb.ru
4. Выполнить команду vagrant up и дождаться полного выполнения задания
5. В командной строке выполнить:
	vagrant ssh control (в качестве логина использовать vagrant)
	
6. попав в терминал ВМ control выполнить следующие команды:
	sudo su ansible
	cd ~/ansible/
	ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
	ssh-copy-id node1.example.com && ssh-copy-id node2.example.com
		ответить дважды (yes, password)
7. Запускаем первый плэйбук, выполнив команду: 
	ansible-playbook /home/ansible/ansible/task1.yml -i inventory
8. По окончании запускаем второй плэйбук, выполнив команду:
	ansible-playbook /home/ansible/ansible/task2.yml -i inventory
9. По окончании открыть в браузере страничку: https://192.168.56.111/

10. В командной строке два раза выполнить exit
11. Вернувшись в консоль хоста выполнить vagrant ssh node2
12. попав в терминал ВМ node2 выполнить следующие команды:
	sudo su ansible
	ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
	ssh-copy-id node1.example.com
		ответить (yes, password)
13. тестово залогиниться на node1
	ssh ansible@node1
14. Выполнить sudo cat /var/lib/jenkins/secrets/initialAdminPassword для получения пароля для установки jenkins