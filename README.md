# ERPNext-installation-Guide
## The complete guide to install ERPNext in your Ubuntu system 22.04.31

### Pre-requisites 

      Python 3.11+
      Node.js 20+
      Redis 5                                       (caching and real time updates)
      MariaDB 10.3.x / Postgres 9.5.x               (to run database driven apps)
      yarn 1.12+                                    (js dependency manager)
      pip 20+                                       (py dependency manager)
      wkhtmltopdf (version 0.12.5 with patched qt)  (for pdf generation)
      cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)
      NGINX                                         (proxying multitenant sites in production)

### STEP 0 Create a bench user called (frappe)
sudo adduser frappe
usermod -aG sudo frappe
su frappe 
cd /home/frappe

### STEP 1 Install git
Git is the most commonly used version control system. Git tracks the changes you make to files, 
so you have a record of what has been done, and you can revert to specific versions should you ever need to. 
Git also makes collaboration easier, allowing changes by multiple people to all be merged into one source.
    
    sudo apt-get install git 
    chmod -R o+rx /home/frappe

### STEP 2 install python-dev
python-dev is the package that contains the header files for the Python C API, 
which is used by lxml because it includes Python C extensions for high performance.

    sudo apt-get install python3-dev

### STEP 3 Install setuptools and pip (Python's Package Manager).
Setuptools is a collection of enhancements to the Python distutils that allow developers 
to more easily build and distribute Python packages, especially ones that have 
dependencies on other packages. Packages built and distributed using setuptools 
look to the user like ordinary Python packages based on the distutils.

pip is a package manager for Python.  It's a tool that allows you to install and manage 
additional libraries and dependencies that are not distributed as part of the standard library.

    sudo apt-get install python3-setuptools python3-pip python3-distutils

### STEP 4 Install virtualenv
virtualenv is a tool for creating isolated Python environments containing their own copy of
python , pip , and their own place to keep libraries installed from PyPI.
It's designed to allow you to work on multiple projects with different dependencies 
at the same time on the same machine.
    
    sudo apt-get install virtualenv
    
  CHECK PYTHON VERSION 
  
    python3 -V
  
  IF VERSION IS 3.10.X RUN
  
    sudo apt install python3.10-venv

  IF VERSION IS 3.11.X RUN
  
     sudo apt install python3.11-venv

### STEP 5 Install MariaDB stable package
MariaDB is developed as open source software and as a relational database it provides an SQL interface 
for accessing data.

   sudo apt install mariadb-server mariadb-client
   sudo apt update
       
IMPORTANT :During this installation you'll be prompted to set the MySQL root password.
If you are not prompted for the same You can initialize the MySQL server setup by executing 
the following command
    
    sudo mysql_secure_installation
    
### STEP 6  MySQL database development files

    sudo apt-get install libmysqlclient-dev

    When you run the above command, the server will show the following prompts.

Enter current password for root: (Enter blank if its first time installation)
Switch to unix_socket authentication [Y/n]: Y
Change the root password? [Y/n]: Y
It will ask you to set new MySQL root password at this step. This can be different from the SSH root user password.
Remove anonymous users? [Y/n] Y
Disallow root login remotely? [Y/n]: N
Remove test database and access to it? [Y/n]: Y
Reload privilege tables now? [Y/n]: Y

### STEP 7 Edit the mariadb configuration ( unicode character encoding )

    sudo nano /etc/mysql/my.cnf

add this to the my.cnf file

    [mysqld]
    character-set-client-handshake = FALSE
    character-set-server = utf8mb4
    collation-server = utf8mb4_unicode_ci

    [mysql]
    default-character-set = utf8mb4

Now press (Ctrl-X) + (Y) to exit

    sudo service mysql restart

### STEP 8 install Redis
Redis is an open source (BSD licensed), in-memory data structure store, used as a database, 
cache, and message broker.
    
    sudo apt-get install redis-server

### STEP 9 install Node.js 20.X package
Node.js is an open source, cross-platform runtime environment for developing server-side and 
networking applications. Node.js applications are written in JavaScript, and can be run within the Node.js
runtime on OS X, Microsoft Windows, and Linux.

    sudo apt-get install curl
    curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt-get install -y nodejs

### OR    
    
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    source ~/.profile
    nvm install 20

### STEP 10  install Yarn and Npn
Yarn is a JavaScript package manager that aims to be speedy, deterministic, and secure. 
See how easy it is to drop yarn in where you were using npm before, and get faster, more reliable installs.
Yarn is a package manager for JavaScript.
    
    sudo npm install -g yarn

### STEP 11 install wkhtmltopdf
Wkhtmltopdf is an open source simple and much effective command-line shell utility that enables 
user to convert any given HTML (Web Page) to PDF document or an image (jpg, png, etc)

    sudo apt-get install xvfb libfontconfig wkhtmltopdf
    
### if you have to setup production server go to Step 16 else continue 

### STEP 12 install frappe-bench

    sudo -H pip3 install frappe-bench
    
IMPORTANT: you may wish to log out and log back into your terminal 
before next step and You must login.
    
    bench --version
    yarn add node-sass
    
### STEP 13 initilise the frappe bench & install frappe latest version 

    bench init frappe-bench --frappe-branch version-15
    
    cd frappe-bench
    
    bench start
    
### STEP 14 create a site in frappe bench 
    
    bench new-site site.1

### STEP 15 install ERPNext latest version in bench & site (site1.local) wich is the default site name that Frappe and ErpNext uses

    bench get-app erpnext --branch version-15
    
    ###OR
    
    bench get-app https://github.com/frappe/erpnext --branch version-15

### Begin to install the software you want on your deploy

    bench --site site1.local install-app erpnext 
    
   
bench get-app hrms --resolve-deps
bench --site site1.local install-app hrms 

bench get-app payments --resolve-deps
bench --site site1.local install-app payments

bench get-app lending --resolve-deps
bench --site site1.local install-app lending 

bench get-app builder --resolve-deps
bench --site site1.local install-app builder

bench get-app print_designer --resolve-deps
bench --site site1.local install-app print_designer

bench get-app lms --resolve-deps
bench --site site1.local install-app lms

bench get-app gameplan --resolve-deps
bench --site site1.local install-app gameplan

bench get-app webshop --resolve-deps
bench --site site1.local install-app webshop

bench get-app crm --resolve-deps
bench --site site1.local install-app crm

bench get-app insights --resolve-deps
bench --site site1.local install-app insights

bench get-app ecommerce_integrations --resolve-deps
bench --site site1.local install-app ecommerce_integrations

bench get-app helpdesk --resolve-deps
bench --site site1.local install-app helpdesk

bench get-app wiki --resolve-deps
bench --site site1.local install-app wiki

bench get-app erpnext-shipping --resolve-deps
bench --site site1.local install-app erpnext_shipping

bench get-app erpnext_price_estimation --resolve-deps
bench --site site1.local install-app erpnext_price_estimation

bench get-app exotel_integration --resolve-deps
bench --site site1.local install-app exotel_integration

bench get-app waba_integration --resolve-deps
bench --site site1.local install-app waba_integration

bench get-app library_management --resolve-deps
bench --site site1.local install-app library_management

bench get-app healthcare --resolve-deps
bench --site site1.local install-app healthcare

bench get-app agriculture --resolve-deps
bench --site site1.local install-app agriculture

bench get-app hospitality --resolve-deps
bench --site site1.local install-app hospitality

bench get-app india-compliance --resolve-deps
bench --site site1.local install-app india_compliance
    
    bench migrate
    
    bench start



### Optional step for cratetind production setup

### Step 16 setup to production

bench set-config -g developer_mode 0
bench switch-to-master --upgrade
bench clear-cache
bench clear-website-cache
bench setup supervisor
bench setup production
bench setup supervisor
sudo supervisorctl reread
sudo supervisorctl reload
sudo supervisorctl restart all
bench migrate
bench restart


## If you installed with --production option and you want to stop production services, and start in develop mode

      sudo service supervisor stop
      sudo service redis stop
      sudo service nginx stop

### Also if you need to set from production to develop

cd /frappe-bench/sites/site1.local/site_config.json
bench set-config -g developer_mode 1
bench --site site1.local clear-cache
bench setup requirements --dev
sudo bench setup production site1.local-frappe
bench migrate
bench restart

###  To start in develop mode, you need to have Procfile in the frappe-bench directory.

      bench setup procfile
      
### To start develop server:   
      bench start

Open the 0.0.0.0 or server IP in web browser and login to production server
    
   
  #### Port cofiguration for multiple site
    
    
    Switch on/off DNS based multitenancy (once)

      bench config dns_multitenant on/off

    Create a new site

      bench new-site site2.local

    Set port (82)

       bench set-nginx-port site2.local 82

    regenerate nginx config

       bench setup nginx

    reload nginx

      sudo service nginx reload
      
    reload supervisor
      sudo service supervisor restart
          
### others

      sudo add-apt-repository ppa:certbot/certbot
      sudo apt update
      sudo apt install python-certbot-nginx
      sudo certbot --nginx -d site1.local
      
      ./env/bin/python -m pip install -q -U -e /apps/frappe
              
### To restart the production mode agin

      sudo service supervisor start
      sudo service redis start
      sudo service nginx start
 
### To check on the status of services - mainly whether they are active or not

      sudo service supervisor status
      sudo service redis status
      sudo service nginx status


    
    
