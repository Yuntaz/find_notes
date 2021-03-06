# How to enable the HTTPS on Find

## Install Certbot

```
sudo yum -y install certbot
```

## Create a directory to store the Java keystore
```
sudo mkdir -p /etc/ssl/private/
```

## Generate the Keystore
Keystore can't be created empty, so we need to create a dummy key on it.
```
sudo keytool -genkey -keyalg RSA -alias Find -keystore /etc/ssl/private/keystore.jks -validity 3600 -keysize 2048
```
You can be asked some questions about the Certificate. Just give the answers like this
```
CN=demo.deepondata.com, OU=www.deepondata.com, O=DeepOnData, L=Miami, ST=Florida, C=US
```
## Use certbot to create a SSL Certificate
We need to have an A redirect to our subdomain, for example, demo.deepondata is an A record to the IP of the droplet.
```
sudo certbot certonly --standalone --preferred-challenges http -d demo.deepondata.com
```
## Import the new certificate into the Ketstore
```
sudo openssl pkcs12 -export -out /etc/ssl/private/keystore.pkcs12 -in /etc/letsencrypt/live/demo.deepondata.com/fullchain.pem -inkey /etc/letsencrypt/live/demo.deepondata.com/privkey.pem
sudo keytool -importkeystore -srckeystore /etc/ssl/private/keystore.pkcs12 -srcstoretype PKCS12 -destkeystore /etc/ssl/private/keystore.jks
```
## Give the arguments to Find to start with SSL
```
java -Dserver.ssl.key-store=/etc/ssl/private/keystore.jks -Dserver.ssl.key-store-password=MyPassword -Dserver.ssl.key-password=KeyPassword -Didol.find.home=[home directory] -Dserver.port=443 -jar find.war -uriEncoding utf-8
```

## Redirect port 80 to 443
We need to install apache only on the port 80, so we need to remove welcome.conf and ssl.conf which are by default and we need to create a non-ssl.conf doing the redirect.
```
sudo yum -y install mod_ssl
rm /etc/httpd/conf.d/ssl.conf
rm /etc/httpd/conf.d/welcome.conf
sudo bash -c 'echo "<VirtualHost *:80>" >  /etc/httpd/conf.d/non-ssl.conf'
sudo bash -c 'echo -e "\tServerName demo.deepondata.com" >>  /etc/httpd/conf.d/non-ssl.conf'
sudo bash -c 'echo -e "\tRedirect "/" "https://demo.deepondata.com/"" >>  /etc/httpd/conf.d/non-ssl.conf'
sudo bash -c 'echo "</VirtualHost>" >>  /etc/httpd/conf.d/non-ssl.conf'
sudo systemctl start httpd
```


