1. yum install httpd mod_ssl

2. create private key and self signed certificate:
mkdir /etc/httpd/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/httpd/ssl/apache.key -out /etc/httpd/ssl/apache.crt

3. `vi /etc/httpd/conf.d/ssl.conf` and update following:
<VirtualHost 10.66.208.170:443>
DocumentRoot "/var/www/html"
ServerName example1.com:443
SSLCertificateFile /etc/httpd/ssl/apache.crt
SSLCertificateKeyFile /etc/httpd/ssl/apache.key

4. comment out the whole file of /etc/httpd/conf.d/welcome.conf

5. systemctl restart httpd
