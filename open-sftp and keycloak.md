Install: https://www.keycloak.org/getting-started/getting-started-zip
TLS: https://www.keycloak.org/server/enabletls

On a new VM:
* sudo add-apt-repository ppa:openjdk-r/ppa
* sudo apt update && sudo apt upgrade
* sudo apt install unzip openjdk-21-jdk # -jre instead of -jdk if you don't care about java dev
* wget https://github.com/keycloak/keycloak/releases/download/26.0.5/keycloak-26.0.5.zip

https://www.keycloak.org/getting-started/getting-started-zip

export KC_BOOTSTRAP_ADMIN_USERNAME=$(tr -dc A-Za-z0-9 </dev/urandom | head -c 13;)
export KC_BOOTSTRAP_ADMIN_PASSWORD=$(tr -dc A-Za-z0-9 </dev/urandom | head -c 13;)
echo "username $KC_BOOTSTRAP_ADMIN_USERNAME"
echo "password $KC_BOOTSTRAP_ADMIN_PASSWORD"
bin/kc.sh start-dev

```
sudo apt-add-repository ppa:certbot/certbot
```
certbot certonly -d keycloak.briansweeney.dev \
--preferred-challenges dns-01 --manual -m \<my email\> \
--work-dir /home/$USER/certbot/work \
--config-dir /home/$USER/certbot/config \
--logs-dir /home/$USER/certbot/logs


# SFTPGo
```
sudo add-apt-repository ppa:sftpgo/sftpgo 
sudo apt update 
sudo apt install sftpgo
```
create /etc/sftpgo/env.d/httpd.env and re-use the cert from above

```shell
SFTPGO_HTTPD__BINDINGS__0__PORT=9443
SFTPGO_HTTPD__BINDINGS__0__ENABLE_HTTPS=1
SFTPGO_HTTPD__BINDINGS__0__CERTIFICATE_FILE="/var/lib/sftpgo/certs/sftpgo.com.crt"
SFTPGO_HTTPD__BINDINGS__0__CERTIFICATE_KEY_FILE="/var/lib/sftpgo/certs/sftpgo.com.key"
```
* https://docs.sftpgo.com/latest/config-file/#http-server
* https://docs.sftpgo.com/latest/env-vars/

