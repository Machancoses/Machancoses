# Logs centralizados
## Introducción
El trabajo del administrador de sistemas puede verse simplificado centralizando los logs.
## Desarrollo
1. Para empezar tenemos que editar en el servidor, el archivo /etc/rsyslog.conf. 

2. En el cliente hay que poner la ip del servidor junto al puerto en el archivo **/etc/rsyslog.conf**. En este caso: *.* @@192.168.99.160:514

```bash
#$ActionResumeRetryCount -1    # infinite retries if host is down
# remote host is: name/ip:port, e.g. 192.168.0.1:514, port optional
#*.* @@remote-host:514
*.* @@192.168.0.160:514
# ### end of the forwarding rule ###
```


3. Tenemos que descomentar las líneas "ModLoad"y "UDPServerRun".


 ```bash
# Provides UDP syslog reception
$ModLoad imudp
$UDPServerRun 514
 
# Provides TCP syslog reception
$ModLoad imtcp
$InputTCPServerRun 514

 ```
4. El siguiente paso es habilitar en el servidor, el puerto 514 para el protocolo de red TCP.

```text
sudo ufw allow 514/tcp
```

 5. Por último queda reiniciar el servidor y probar el funcionamiento.

```
sudo systemctl restart rsyslog
```

### Comprobación

Tenemos que lanzar un log con el comando `logger`

Iremos al registro **/var/log/syslog** donde saldrán los logs que nos han mandado
