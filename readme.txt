

sudo apt upgrade
sudo apt update
sudo apt install apache2
sudo apt install mysql-server
sudo apt install php libapache2-mod-php php-mysql

2. Configurer MySQL

Lance le script de sécurité de MySQL :

bash

sudo mysql_secure_installation

Suis les instructions pour sécuriser ton installation.
3. Créer une base de données pour WordPress

Connecte-toi à MySQL :

bash

sudo mysql -u root -p

Ensuite, crée une base de données et un utilisateur :

sql

CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'Azerty123!';

GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;

Remplace motdepasse par un mot de passe sécurisé.
4. Télécharger WordPress

Télécharge WordPress et décompresse-le :

bash

cd /tmp
wget https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz

5. Copier les fichiers WordPress

Copie les fichiers dans le répertoire d'Apache :

bash

sudo cp -a /tmp/wordpress/. /var/www/html/

Exécutez la commande suivante pour déplacer tout le contenu du répertoire /var/www/html/wordpress vers /var/www/html :

bash

sudo mv /var/www/html/wordpress/* /var/www/html/
sudo mv /var/www/html/wordpress/.* /var/www/html/  # Pour déplacer aussi les fichiers cachés

Supprimez la page par défaut d'Apache : Si la page par défaut d'Apache (index.html) est toujours présente dans /var/www/html/, elle sera chargée à la place de WordPress. Supprimez ce fichier :

bash

sudo rm /var/www/html/index.html


Cette commande va déplacer tous les fichiers et répertoires, y compris les fichiers cachés (comme .htaccess), dans le répertoire html.
2. Supprimez le dossier vide wordpress

Une fois les fichiers déplacés, vous pouvez supprimer le dossier vide wordpress :

bash

sudo rmdir /var/www/html/wordpress


6. Configurer les permissions

Assure-toi que les fichiers sont accessibles par Apache :

bash

sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/

7. Configurer WordPress

Renomme le fichier de configuration :

bash

cd /var/www/html/
 cd wordpress
 sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php

Modifie les lignes suivantes pour ajouter les informations de ta base de données :

php

define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'Azerty123!');


<VirtualHost *:80>
    ServerName wordpress-extra.julien-hennebo.cloudns.be
    DocumentRoot /var/www/html  # Point vers le répertoire html

    <Directory /var/www/html>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Activez le site et le module rewrite : Exécutez les commandes suivantes pour activer le site et le module de réécriture, si ce n'est pas déjà fait :
sudo a2ensite wordpress-extra.conf
sudo a2enmod rewrite
sudo systemctl reload apache2


8. Finaliser l’installation

Accède à l’adresse de ton serveur dans un navigateur (http://localhost ou http://ton_ip) et suis les instructions d’installation de WordPress.
9. (Optionnel) Installer un certificat SSL

Pour sécuriser ton site, tu peux installer un certificat SSL avec Certbot.

Si tu as besoin de plus d'informations sur un des points, n'hésite pas à demander !


ChatGPT peut faire des erreurs. Envisagez de vérifier les informations importantes.
