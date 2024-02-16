# Proxy_Inverso

# Instalar Nginx

---

1. Instalamos la versión de Nginx para windows de la web oficial: https://nginx.org/en/download.html
2. Ejecutamos el .exe del zip que se ha descargado y le permitimos a windows acceder a la apliación.

# Archivo Nginx.conf

---

- **server_name  [alvarodelburgoperez.com](http://alvarodelburgoperez.com);** (podemos poner el nombre que queramos, importante añadir el “.com” tanto en el server name como en el archivo **hosts** de Windows)
- **root C:/Users/alvar/Desktop/DevOps/entorno_mkdocs/pagina_mkdocs/site;** (esta es la ruta donde se ha generado el html en el proyecto de mkdocs)
- **proxy_pass http://127.0.0.1:8080;** (esta es la ip que hemos añadido en el archivo **hosts**)

```jsx
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  alvarodelburgoperez.com;  # Reemplaza "tudominio.com" con tu dominio

        location / {
            root C:/Users/alvar/Desktop/DevOps/entorno_mkdocs/pagina_mkdocs/site;  # Ruta absoluta a tu proyecto MkDocs
            index index.html;
            try_files $uri $uri/ /index.html;
        }

        location /proxy/ {
            proxy_pass http://127.0.0.1:8080;  # Cambia la dirección y el puerto del servidor backend si es necesario
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_redirect     off;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

# Archivo hosts en Windows

---

1. Accedemos a la ruta **C:\Windows\System32\drivers\etc\hosts** y se nos abre este archivo: 

```jsx
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost
    127.0.0.1       alvarodelburgoperez.com
```

1. Añadimos la línea **127.0.0.1       [alvarodelburgoperez.com](http://alvarodelburgoperez.com) (**que corresponde a la ip y el nombre del host).

# Reiniciar Nginx

---

1. Nos metemos en la ruta donde esta el nginx.exe desde el PowerShell (**C:\Users\alvar\Desktop\DevOps\nginx-1.25.3**) ejecutado con permisos de administrador.
2. Ejecutamos el comando **./nginx -s reload**
