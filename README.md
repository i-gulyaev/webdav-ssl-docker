# WebDAV/SSL in Docker
Provides WebDAV access via SSL. You need to prepare your own password
file and provide your own certificate/key.

This repository has been originally forked from https://github.com/phylor/webdav-ssl-docker

## Create Password File

    htpasswd -c htpasswd username

## Certificates
You require a server certificate and a corresponding key for the SSL
connection. They need to be in a directory and called `web.crt` and
`web.key`.

    openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout web.key -out web.crt

## Exposed Volumes

- `/certs`: Containing server certificate and key for SSL
- `/htpasswd`: Htpasswd file for DAV authentication
- `/var/www/webdav`: DAV content

## Run Container

    docker run -d --name webdav-ssl -v /path/to/htpasswd:/htpasswd -v /path/to/certs:/certs -v /path/to/data:/var/www/webdav webdav-ssl

## Test
    curl -k --user username:'password' -T filename https://localhost/webdav/filename
    
