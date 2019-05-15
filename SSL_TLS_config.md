Here are some security recommendations for a better SSL/TLS configuration of Apache.

# Some Alignment
Yes, I recommend adjust in this file first with the hardest configset and flexibilize something for the specific VHost, when necessary.
For this configurations, I am thinking firstly in security, then in performance. Sure, you have to scale some configs if you have a high load server.
This configuration was done for a small Virtual Machine with CentOS 7. I removed the comments for better reading, but I strongly recommend you read it in your server or in the [official documentation](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html).

## /etc/httpd/conf.d/ssl.conf
Listen 443 https  
SSLPassPhraseDialog exec:/usr/libexec/httpd-ssl-pass-dialog  
SSLSessionCache         shmcb:/run/httpd/sslcache(512000)  
SSLSessionCacheTimeout  300  
SSLRandomSeed startup file:/dev/urandom  256  
SSLRandomSeed connect builtin  
SSLCryptoDevice builtin  

`##`  
`## SSL Virtual Host Context`  
`##`  

<VirtualHost _default_:443>  
`#### Some other options that could be safely left in default. ####`  
`...`  

ErrorLog logs/ssl_error_log  
TransferLog logs/ssl_access_log  
LogLevel warn  

SSLEngine on  
`# https://httpd.apache.org/docs/2.4/mod/mod_ssl.html`  
SSLProtocol all -SSLv2 -SSLv3 -TLSv1 -TLSv1.1  

`# https://httpd.apache.org/docs/2.4/mod/mod_ssl.html#sslciphersuite`  
SSLCipherSuite HIGH:3DES:!aNULL:!MD5:!SEED:!IDEA  
**SSLHonorCipherOrder on**  
`...`  
`#### Many other options that could be safely left in default. ####`  
</VirtualHost>  

## Restart  
This settings require a restart.  
> systemctl restart httpd  
