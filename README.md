# IaC (Infrastructire as a Code) #

IaC merupakan salah satu teknologi cloud dengan tujuan untuk automasi deployment dari suatu environment system. Segala sesuatu terkait deployment mulai dari preparasi environment hingga setup sistem dilakukan berdasrkan script code yang telah dibuat melalui tool IaC. Ada beberapa tool IaC yang cukup populer, salah satunya yaitu Juju.

# Juju #

Juju adalah salah satu tool IaC yang disediakan di sistem operasi Linux Ubuntu. Dengan menggunakan juju proses deployment bisa dilakukan dengan beberapa command yang sederhana. Dengan menggunakan juju kita bisa mendeploy aplikasi baik di environment local maupun di environemnt cloud seperti di E2 AWS, Openstack, Digital Ocean dll.

# Menggunakan Juju #

Contoh yang akan dijelaskan dibawah adalah langkah-langkah serta instalasi skema deplyment local. Untuk machine yang akan digunakan untuk menampung deployment adalah container yang dibuat dengan menggunakan LXC.

# Instalasi LXC (Linux Container) #

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

Agar support dengan LXC kita perlu menambahkan satu konfigurasi LXC pada file environments.yml di bagian konfigurasi local, konfigurasinya adalah sebagai berikut.

	local:
    	type: local
    	lxc-clone: true

Kita tambahkan lxc-clone: true untuk mendefinisikan bahwa deployment akan dilakukan di container yang dibuat dengan LXC.

Selanjutnya kita inisiasi service juju dengan command berikut

	$ juju bootstrap

Setelah itu kita coba deploy service wordpress dengan menggunakan database mysql
	
	$ juju deploy wordpress
	$ juju deploy mysql

Selanjutnya kita buat realsi antara kedua service tersebut.

	$ juju add-relation wordpress mysql

Setelah itu kita expose service wordpress agar dapat diakses dengan command

	$ juju expose wordpress

# Monitoring Status Service #

Untuk melakukan pengecekan terhadap deployment yang sudah dilakukan kita gunakan command berikut
	
	$ juju status

Output command tersebut akan menampilkan detail dari setiap intance service yang telah dibentuk disertai dengan status dari setiap service yang telah dibuat. Berikut adalah contoh outputnya.

	services:
  		mysql:
    		charm: cs:precise/mysql-18
		    exposed: false
		    relations:
		      db:
		      - wordpress
		    units:
		      mysql/0:
			...
        	public-address: 10.0.3.110
  		wordpress:
		    charm: cs:precise/wordpress-12
		    exposed: true
		    relations:
		      db:
		      - mysql
		      loadbalancer:
		      - wordpress
		    units:
		      wordpress/0:
			...
		     public-address: 10.0.3.115

Dari output status tersebut kita bisa melihat ip address di setiap service, untuk mengakses servicenya kita bisa mengakses ip tersebut.

# Menghapus Environment #

Untuk menghapus environment eksekusi command berikut :

	$ juju destroy-environment <environment>

Dalam contoh di atas jika kita ingin menghapus environment loal maka commandnya adalah 
	
	juju destroy-environment local

