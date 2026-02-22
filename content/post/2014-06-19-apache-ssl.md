---
title: '[Apache] SSL (self-signed & purchased version) '
description: "[Self-signed] 1. Generate a host key: sudo ssh-keygen -f host.key 2. Generate a certificate request file sudo openssl req -new -key host."
date: 2014-06-19
categories: ['DevOps']
---


## [Self-signed]

1. Generate a host key: 
```
sudo ssh-keygen -f host.key
```
 
2. Generate a certificate request file
```
sudo openssl req -new -key host.key -out request.csr
```

Type what you want:
```
  Country Name (2 letter code) [AU]:TW
  State or Province Name (full name) [Some-State]:Taiwan
  Locality Name (eg, city) []:Taipei
  Organization Name (eg, company) [Internet Widgits Pty Ltd]:
  Organizational Unit Name (eg, section) []:
  Common Name (e.g. server FQDN or YOUR name) []:
  Email Address []:
  
  Please enter the following 'extra' attributes
  to be sent with your certificate request
  A challenge password []:
  An optional company name []:
```

3. Create the SSL certificate
`sudo openssl x509 -req -days 365 -in request.csr -signkey host.key -out server.crt`

	Create a nopass' key (optional)
	`openssl rsa -in host.key -out host.nopass.key`

4. Configure Apache
`LoadModule ssl_module libexec/apache2/mod_ssl.so`

```
SSLEngine on 
SSLCertificateFile "/etc/apache2/ssl/server.crt"
SSLCertificateKeyFile "/etc/apache2/ssl/host.nopass.key"
```

```
sudo apachectl configtest 
sudo apachectl restart 
```

p.s. enable mods: `sudo a2enmod ssl`
enable sites: `sudo a2ensite default-ssl`
 
 
## [Purchased (Symantec)]
---
[http://www.symantec.com/tv/products/details.jsp?vid=1452855338001]
<object id="flashObj" width="640" height="360" classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=9,0,47,0"><param name="movie" value="http://c.brightcove.com/services/viewer/federated_f9?isVid=1&isUI=1" /><param name="bgcolor" value="#FFFFFF" /><param name="flashVars" value="videoId=1452855338001&linkBaseURL=http%3A%2F%2Fwww.symantec.com%2Ftv%2Fproducts%2Fdetails.jsp%3Fvid%3D1452855338001&playerID=1170996384001&playerKey=AQ~~,AAAABuIiy9k~,I8BhasVwr9yry4vRRPOpsSrBvwjFZ03K&domain=embed&dynamicStreaming=true" /><param name="base" value="http://admin.brightcove.com" /><param name="seamlesstabbing" value="false" /><param name="allowFullScreen" value="true" /><param name="swLiveConnect" value="true" /><param name="allowScriptAccess" value="always" /><embed src="http://c.brightcove.com/services/viewer/federated_f9?isVid=1&isUI=1" bgcolor="#FFFFFF" flashVars="videoId=1452855338001&linkBaseURL=http%3A%2F%2Fwww.symantec.com%2Ftv%2Fproducts%2Fdetails.jsp%3Fvid%3D1452855338001&playerID=1170996384001&playerKey=AQ~~,AAAABuIiy9k~,I8BhasVwr9yry4vRRPOpsSrBvwjFZ03K&domain=embed&dynamicStreaming=true" base="http://admin.brightcove.com" name="flashObj" width="640" height="360" seamlesstabbing="false" type="application/x-shockwave-flash" allowFullScreen="true" allowScriptAccess="always" swLiveConnect="true" pluginspage="http://www.macromedia.com/shockwave/download/index.cgi?P1_Prod_Version=ShockwaveFlash"></embed></object>

<object id="flashObj" width="640" height="360" classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=9,0,47,0"><param name="movie" value="http://c.brightcove.com/services/viewer/federated_f9?isVid=1&isUI=1" /><param name="bgcolor" value="#FFFFFF" /><param name="flashVars" value="videoId=1452855338001&linkBaseURL=http%3A%2F%2Fwww.symantec.com%2Ftv%2Fproducts%2Fdetails.jsp%3Fvid%3D1452855338001&playerID=1170996384001&playerKey=AQ~~,AAAABuIiy9k~,I8BhasVwr9yry4vRRPOpsSrBvwjFZ03K&domain=embed&dynamicStreaming=true" /><param name="base" value="http://admin.brightcove.com" /><param name="seamlesstabbing" value="false" /><param name="allowFullScreen" value="true" /><param name="swLiveConnect" value="true" /><param name="allowScriptAccess" value="always" /><embed src="http://c.brightcove.com/services/viewer/federated_f9?isVid=1&isUI=1" bgcolor="#FFFFFF" flashVars="videoId=1452855338001&linkBaseURL=http%3A%2F%2Fwww.symantec.com%2Ftv%2Fproducts%2Fdetails.jsp%3Fvid%3D1452855338001&playerID=1170996384001&playerKey=AQ~~,AAAABuIiy9k~,I8BhasVwr9yry4vRRPOpsSrBvwjFZ03K&domain=embed&dynamicStreaming=true" base="http://admin.brightcove.com" name="flashObj" width="640" height="360" seamlesstabbing="false" type="application/x-shockwave-flash" allowFullScreen="true" allowScriptAccess="always" swLiveConnect="true" pluginspage="http://www.macromedia.com/shockwave/download/index.cgi?P1_Prod_Version=ShockwaveFlash"></embed></object>
