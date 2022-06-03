# BLocage crawler

<h4>Recherchez dans le fichier journal Apache une liste d'agents utilisateurs </h4>
 
        cat /var/log/apache2/access.log | cut -d '"' -f6 | grep -v -E -i "mozilla|chrome|safari|opera" |

<h4>Une liste d'utilisateurs-agents suspects ayant accés a votre site Web sera affichée </h4>

         GumGum-Bot/1.0 (http:gumgum.com; support@gumgum.com)
         PostmanRuntime/7.19.0
         ag_dm_spider v1.0
         Microsoft Office Word 2014
         Slackbot 1.0 (+https://api.slack.com/robots)
         Slackbot-LinkExpanding 1.0 (+https://api.slack.com/robots)
         WebexTeams
         Chimebot
         www.ru
         facebookexternalhit/1.1 (+http://www.facebook.com/externalhit_uatext.php)
         admantx-sap/2.4 (+http://www.admantx.com/service-fetcher.html)
         Scrapy/2.4.1 (+https://scrapy.org)
         ias-sg/3.1(+https://www.admantx.com/service-fetcher.html)
         WhatsApp/2.21.7.14 A

<h4>Créez une liste d'agents utilisateur à bloquer</h4>

         GumGum-Bot
         PostmanRuntime
         ag_dm_spider
         Scrapy
         Chimebot

<h4>Activez les modules Apache Requis</h4>

        a2nmod rewrite

<h4>Modifiez le fichier de configuration Apache pour le site Web par défaut</h4>

         vi /etc/apache2/sites-enabled/000-default.conf

<h4>Ajoutez les lignes suivantes a ce fichier de configuration</h4>

         RewriteEngine On
         RewriteCond %{HTTP_USER_AGENT} (gumgum-bot|postmanruntime|ag_dm_spider|scrapy|chimebot) [NC]
         RewriteRule .* - [F,L]

<h4>Modifiez les valeurs USER-AGENT pour refleter vos besoins</h4>

         RewriteCond %{HTTP_USER_AGENT} (gumgum-bot|postmanruntime|ag_dm_spider|scrapy|chimebot) [NC]

<h4>Le fichier,avant notre configuration</h4>

         <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/html
            ErrorLog ${APACHE_LOG_DIR}/error.log
            Customlog ${APACHE_LOG_DIR}/access.log combined
         </Virtualhost>

<h4>Et voici le fichier apres notre configuration</h4>

         <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/html
            ErrorLog ${APACHE_LOG_DIR}/error.log
            Customlog ${APACHE_LOG_DIR}/access.log combined
         RewriteEngine On
         RewriteCond %{HTTP_USER_AGENT} (gumgum-bot|postmanruntime|ag_dm_spider|scrapy|chimebot)
         RewriteRule .* - [F,L]
         </Virtualhost>

<h4>Redemarrez le service Apache</h4>

         service apache2 restart

Dans notre exemple, le serveur apache interdra l'accés à une liste de bots et crawlers sélectionnés par l'administrateur

<h4> A partir d'un ordinateur Linux distant testez votre configuration</h4>

         curl -I -A "GumGum-Bot" http:/www.gameking.tips

<h4> Voici la sortie de commande</h4>

         HTTP/1.1 403 Forbidden 
         Date: Sat, 03 June 2022 18:49:39 GMT
         Server: Apache/2.4.41 (Ubuntu)
         Content-Type: text/html; charset=iso-8859-1

Le serveur indiquera l'accès à partir de valeurs UTILISATEUR-AGENT spécifiques

<h4>A partir d'un ordinateur Linux distant, essayer d'effectuer l'accès en utilisant n'importe quelle autre valeur USER-AGENT</h4>

         curl -I -A "test" http://www.gameking.tips

<h4>Voici la sortie de commande</h4>

         HTTP/1.1 200 OK
         Date: Sat, 03 June 2022 18:49:39 GMT
         Server: Apache/2.4.41(Ubuntu)
         Last-Modified: Sat, 03 June 2022 17:59:03 GMT
         ETag: "2aa6-5bf1536560652"
         Accept-Ranges: bytes
         Content-Length: 10918
         Vary: Accept-Encoding 
         Content-type: text/html

Le serveur Apache permettra à toute autre valeur USER-AGENT d'acceder a votre site web

<h3>Félicitation! Vous avez appris à configurer le serveur Apache pour refuser l'acces aux mauvais Bots et crawlers </h3>

