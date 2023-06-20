---
-  name: install broadleaf
   host: all
   become: yes
   tasks: 
     - name: install pakages
       ansible.builtin.get_url:
        url:  https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
        dest: packages-microsoft-prod.deb
     - name: install packages-microsoft-prod.deb
        ansible.builtin.shell:
        cmd: sudo apt install packages-microsoft-prod.deb
     - name: install dotnetcor
       apt:
        name:
         - apt-transport-https
         - aspnetcore-runtime-7.0
        update_cache: yes
        state: present
     - name: install nginx
       apt:
        name: nginx
        state: present
     - name: start nginx service
       systemd:
        name: nginx
        state: started
     - name: copy data from default
       copy:
        src: default
        dest: /etc/nginx/sites-available/default
     - name: create directory 
       ansible.builtin.file:
        path: /var/www/nopCommerce
        state: directory
     - name: weget 
       ansible.builtin.get_url:
        url: https://github.com/nopSolutions/nopCommerce/releases/download/release-4.60.3/nopCommerce_4.60.3_NoSource_linux_x64.zip
        dest: /var/www/nopCommerce/nopCommerce_4.60.3_NoSource_linux_x64.zip
     - name: unzip
       ansible.builtin.unarchive:
        src: /var/www/nopCommerce/nopCommerce_4.60.3_NoSource_linux_x64.zip
        dest: /var/www/nopCommerce
        remote_src: yes
     - name: create directory
       ansible.builtin.file:
        path: /var/www/nopCommerce/bin
        state: directory
     - name: create directory
       ansible.builtin.file:
        path: /var/www/nopCommerce/logs
        state: directory
        recurse: yes
        owner: www-data
        group: www-data
     - name: create nop file
       copy:
        src: nop.service
        dest: /etc/systemd/system/nopCommerce.service 
     - name: start nop sevice
       systemd:
        name: nopCommerce
        state: started   


           


       
                 


            