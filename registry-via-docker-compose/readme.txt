cd ssl

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.pem

Utiliser "auth" pour le Common Name

