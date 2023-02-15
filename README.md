# PRC IMAGES (prc-images)

Introduction to PRC IMAGES

## INSTALL

First of all install the [GEOBOX](https://github.com/webmappsrl/geobox) repo and configure the ALIASES command. 

```sh
git clone git@github.com:webmappsrl/prc-images.git
cd prc-images
bash docker/init-docker.sh
geobox_install prc-images
```

Important NOTE: remember to checkout the develop branch.

## Run web server from shell outside docker

In order to start a web server in local environment use the following command:
Replace `${instance name}` with the instance name (APP_NAME in .env file) 

```sh
geobox_serve prc-images
```

### Problemi noti

Durante l'esecuzione degli script potrebbero verificarsi problemi di scrittura su certe cartelle, questo perchè di default l'utente dentro il container è `www-data (id:33)` quando invece nel sistema host l'utente ha id `1000`. Ci sono 2 possibili soluzioni:

-   Chown/chmod della cartella dove si intende scrivere, eg:

    ```bash
      chown -R 33 storage
    ```
    NOTA: per eseguire il comando chown potrebbe essere necessario avere i privilegi di root. In questo caso si deve effettuare l'accesso al cointainer del docker utilizzando lo specifico utente root (-u 0). Questo è valido anche sbloccare la possibilità di scrivere nella cartella /var/log per il funzionamento di Xdedug

-   Utilizzare il parametro `-u` per il comando `docker exec` così da specificare l'id utente, eg come utente root (utilizzare `APP_NAME` al posto di `$nomeApp`):
    `bash
docker exec -u 0 -it php81_$nomeApp bash scripts/deploy_dev.sh
`

Xdebug potrebbe non trovare il file di log configurato nel .ini, quindi generare vari warnings

-   creare un file in `/var/log/xdebug.log` all'interno del container phpfpm. Eseguire un `chown www-data /var/log/xdebug.log`. Creare questo file solo se si ha esigenze di debug errori xdebug (impossibile analizzare il codice tramite breakpoint) visto che potrebbe crescere esponenzialmente nel tempo
