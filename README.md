# Bertelsmann-Scholarship-Introduction-to-Azure-Applications
 Bertelsmann Scholarship - Introduction to Azure Applications

# Create a Resource Group using the Azure CLI
<details><summary>Expand</summary>
<p>

```
az login
az group create --name resource-group-west --location westus2
```

</p>
</details>

# Create a simple web-app with virtual machine
<details><summary>Expand</summary>
<p>

reverse-proxy.conf 
```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
 }
```
```
az login
az vm create \
   --resource-group "resource-group-west" \
   --name "linux-vm-west" \
   --location "westus2" \
   --image "UbuntuLTS" \
   --size "Standard_B1ls" \
   --admin-username "udacityadmin" \
   --generate-ssh-keys \
   --verbose
az vm open-port \
    --port "80" \
    --resource-group "resource-group-west" \
    --name "linux-vm-west"
az vm list-ip-addresses -g resource-group-west -n linux-vm-west
scp -i ~/linux-vm-west_key.pem -r ./web azureuser@52.149.8.45:/home/azureuser
ssh -i ~/linux-vm-west_key.pem azureuser@52.149.8.45
sudo apt-get -y update && sudo apt-get -y install nginx python3-venv
cd /etc/nginx/sites-available
sudo unlink /etc/nginx/sites-enabled/default
sudo vim reverse-proxy.conf 
sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
sudo service nginx restart
cd ~/web
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip && pip install -r requirements.txt
python application.py
exit
```
<img src="/simple-webapp-on-vm-l2e2/lecture2-excercise2.gif">

</p>
</details>

# Create a simple web-app with webapp service
<details><summary>Expand</summary>
<p>


```
az login
az webapp up \
 --resource-group resource-group-west \
 --name hello-world1234 \
 --sku F1 \
 --verbose
az webapp up \
 --name hello-world1234 \
 --verbose
az webapp delete \
    --name hello-world1234 \
    --resource-group resource-group-west
az appservice plan delete \
    --name [App Service Plan Name] \
    --resource-group resource-group-west
```
<img src="/simple-webapp-on-weapp-service-l2e13/lecture2-excercise13.gif">

</p>
</details>

# Azure SQL Database
<details><summary>Expand</summary>
<p>

```
az sql server create \
--admin-user udacityadmin \
--admin-password XXXXX \
--name hello-world-server-udacity \
--resource-group resource-group-west \
--location westus2 \
--enable-public-network true \
--verbose
az sql server firewall-rule create \
-g resource-group-west \
-s hello-world-server-udacity \
-n azureaccess \
--start-ip-address 0.0.0.0 \
--end-ip-address 0.0.0.0 \
--verbose
az sql server firewall-rule create \
-g resource-group-west \
-s hello-world-server-udacity \
-n clientip \
--start-ip-address 99XXXXXXX \
--end-ip-address 99XXsXXXXXX \
--verbose
az sql db create \
--name hello-world-db \
--resource-group resource-group-west \
--server hello-world-server-udacity \
--tier Basic \
--verbose
az sql db delete \
--name hello-world-db \
--resource-group resource-group-west \
--server hello-world-server-udacity \
--verbose
az sql server delete \
--name hello-world-server-udacity \
--resource-group resource-group-west \
--verbose
```
<img src="/Auzre SQL Sotrage/l3e5.gif">

</p>
</details>

# Blob Storage
<details><summary>Expand</summary>
<p>

```
az storage account create \
 --name helloworldudacity \
 --resource-group resource-group-west \
 --location westus2
az storage container create \
 --account-name helloworldudacity \
 --name images \
 --auth-mode login \
 --public-access container
```

<img src="/Auzre SQL Sotrage/l3e8.gif">

</p>
</details>

# Connecting App to Storage
<details><summary>Expand</summary>
<p>

```
https://github.com/HuyDucVo/Bertelsmann-Scholarship-Introduction-to-Azure-Applications/tree/main/Connecting%20App%20to%20Storage
```

<img src="Connecting App to Storage/l3e11.gif">

</p>
</details>
