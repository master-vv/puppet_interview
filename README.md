How it works:
1) please extract the "nginx_proxy.tar" archive to your puppet master node in order to create another module. In my case the path was like this "/etc/puppetlabs/code/environments/production/modules/", and after extraction you should have another folder called ./nginx_proxy with two subfolders called ./files and ./manifests.
    - in the ./files folder you will find the bash script that will be used to configure the nginx on the agent
    - in the ./manifests folder you will find the manifest file for the module
    
 2) Extract the "site.tar" archive to your production main branch on the master node. In my case the path was "/etc/puppetlabs/code/environments/production/manifests". The archive contains the manifest that will call for the module manifest.
 
 3) On the agent host run the command to pull the data from the master node. In my case the command was "/opt/puppetlabs/bin/puppet agent --test"

 4) The puppet agent will pull the config file from the nginx_proxy module and then it will execute the script according to the manifest. The script will do a copy of the default site config file of the nginx engine (/etc/nginx/sites-available/default) and then it will generate a new default config file with all the necessary settings for nginx to run as a reverse proxy for incoming external connections (listening on port 80 and 443) and also as a forward proxy (listening on port 8888) for the incoming internal connections.
 The script will also backup the "/etc/nginx/nginx.conf" file and then add another 2 lines of code in order to define the new parameters for logging the nginx forward proxy traffic.
 
Note: If you want to test the script in a real environment please modify the config.sh file with the correct paths for the ssl certificate. 
