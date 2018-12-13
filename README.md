# IaC (Infrastructire as a Code) #

IaC merupakan salah satu teknologi cloud dengan tujuan untuk automasi deployment dari suatu environment system. Segala sesuatu terkait deployment mulai dari preparasi environment hingga setup sistem dilakukan berdasrkan script code yang telah dibuat melalui tool IaC. Ada beberapa tool IaC yang cukup populer, salah satunya yaitu Juju.

# Juju #

Juju adalah salah satu tool IaC yang disediakan di sistem operasi Linux Ubuntu. Dengan menggunakan juju proses deployment bisa dilakukan dengan beberapa command yang sederhana. Dengan menggunakan juju kita bisa mendeploy aplikasi baik di environment local maupun di environemnt cloud seperti di E2 AWS, Openstack, Digital Ocean dll.

# Menggunakan Juju #

Contoh yang akan dijelaskan dibawah adalah langkah-langkah serta instalasi skema deplyment local. Untuk machine yang akan digunakan untuk menampung deployment adalah container yang dibuat dengan menggunakan LXC.

# Instalasi LXC #

	sudo apt-get install lxc lxctl lxc-templates

# Instalasi Juju #

	$ sudo add-apt-repository ppa:juju/stable
	$ sudo apt-get update
	$ sudo apt-get install juju-local

Untuk membuat IaC secara lokal, kita bisa menginstall package juju-local, namun jika kita ingin mendeploy ke cloud seperti AWS kita harus menginstall package juju-core.

# Konfigurasi Deployment #

Generate config untuk deployment

	$ juju generate-config

Command di atas akan membuat sebuah file konfigurasi deployment juju dengan format .yaml di direktori ~/.juju/environments.yaml yang berisi beberapa pilihan untuk tipe deployment. Untuk memilih tipe deployment local eksekusi command berikut.

	$ juju switch local

