# traefik-pihole
Traefik and pihole for Orange Pi or arm 32 bits processors

Lo primero que hay que hacer es implementar DuckDNS y Let's Encrypts en nuestro HomeAssistant. Para ello sugiero hacer el siguiente tutorial https://domology.es/implentar-duckdns-y-lets-encrypt-en-nuestro-ha/

Una vez hecho eso, podéis seguir este otro tutorial https://domology.es/instalacion-docker-parte-4-traefik/, pero cambiando los archivos traefik.toml y docker-compose por los que yo he creado, y a su vez cambiando dentro de éstos algunos parámetros.

En traefik.toml hay que cambiar: 
- USER:TOKEN_PASSWORD por lo que se ha crado al poner "htpasswd -nb USER PASSWORD", siendo USER tu usuario y PASSWORD tu contraseña.
- DOMAIN por el dominio que hayáis creado en Duckdns.
- EMAIL por tu email.
- IP-OPI por la IP de tu dispositivo. En mi caso es la 192.168.1.100

En docker-compose.yaml hay que cambiar:
- DOMAIN por el dominio que hayáis creado en Duckdns.
- IP-OPI por la IP de tu dispositivo. En mi caso es la 192.168.1.100
- PASSWORD por la contraseña que quieras ponerle.

En mi caso, he puesto en traefik.toml los servicios de homeassistant, motioneye y plex, pero pueden añadirse más  cambiarse por otros.
Si observas éste archivo, puedes ver que en el frontend de motioneye hay algo distinto a los otros:
```
    [frontends.frontend-motioneye.auth]
      [frontends.frontend-motioneye.auth.basic]
        users = ["USER:TOKEN_PASSWORD"]
                insecureSkipVerify = true
 ```
Esto, lo que hace es que cuando entras en la web motioneye.DOMINIO.duckdns.org, te pide el usuario y contraseña que hayas crado. Para Motioeye lo veo necesario, ya que si no, cualquier persona podría ver tu cámara.

## IMPORTANTE

Antes de hacer `docker-compose up -d` os recomiendo hacer lo siguiente, para que no os den errores en el puerto 53:
```
sudo systemctl disable systemd-resolved.service
sudo service systemd-resolved stop
sudo systemctl disable systemd-resolved
cp /etc/resolv.conf /etc/resolv.bak
nano /etc/resolv.conf
```
y cambiar lo que hay en ese archivo por:
```
nameserver 1.1.1.1
nameserver 127.0.0.53
```





