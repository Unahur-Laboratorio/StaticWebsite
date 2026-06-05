# Laboratorio de Sistemas Operativos y Redes

## Instalación del sitio web estático en EC2 Amazon Linux con Apache

Este repositorio incluye el archivo `staticweb.tar` en la raíz. Desde la instancia EC2 podés descargarlo y publicar el sitio en Apache con estos pasos.

### 1. Conectarse a la instancia

```bash
ssh -i /ruta/tu-clave.pem ec2-user@IP_PUBLICA_EC2
```

### 2. Instalar Apache

En Amazon Linux 2023:

```bash
sudo dnf update -y
sudo dnf install -y httpd
```

### 3. Iniciar y habilitar el servicio

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

### 4. Descargar `staticweb.tar` desde GitHub

```bash
wget -O staticweb.tar https://raw.githubusercontent.com/Unahur-Laboratorio/StaticWebsite/master/staticweb.tar
```

Verificar el contenido:

```bash
tar -tf staticweb.tar
```

El archivo contiene:

```text
README.md
index.html
logo.png
script.js
style.css
```

### 5. Descomprimir en el directorio web

```bash
sudo rm -rf /var/www/html/*
sudo tar -xf staticweb.tar -C /var/www/html
```

### 6. Ajustar permisos

```bash
sudo chown -R apache:apache /var/www/html
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;
```

### 7. Reiniciar Apache

```bash
sudo systemctl restart httpd
```

### 8. Abrir el puerto 80 en el Security Group

Agregá una regla de entrada para HTTP por el puerto 80 desde `0.0.0.0/0`.

### 9. Probar en el navegador

Abrí:

```text
http://IP_PUBLICA_EC2
```

### Comando completo

```bash
sudo dnf update -y
sudo dnf install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
wget -O staticweb.tar https://raw.githubusercontent.com/Unahur-Laboratorio/StaticWebsite/master/staticweb.tar
sudo rm -rf /var/www/html/*
sudo tar -xf staticweb.tar -C /var/www/html
sudo chown -R apache:apache /var/www/html
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;
sudo systemctl restart httpd
```