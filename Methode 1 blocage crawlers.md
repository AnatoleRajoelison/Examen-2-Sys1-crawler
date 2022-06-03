# Bloquer les crawlers sur Apache 2

<h4>Accéder dans le dossier log d'Apache </h4>

         cd /var/log/apache2

<h4>Maintenant créeons le fichier qui va s'occuper du blocage crawler</h4>

         nano blocking.sh

<h4>Entrez les scripts suivantes:</h4>

        #!/bin/sh

        cat /var/log/apache2/access.log | grep 'spider\ | bot' | sed 's/[[^\/]*/\./;s/].*$//g' | awk '{print $1}' | uniq | sort | tail -n 100 |iptable -A INPUT $1 -j REJECT;

Alors ceci permet à la fois d'afficher (cat) et de filtrer (grep), et de récupérer des (awk) et de rejeter les requêtes des IP crawlers avec(IPtables).

<h4>Il ne reste plus qu'a donné les droits sur les srcipts</h4>

         chmod +x blocking.sh
        
<h4> Il suffit juste d'executer maintenant</h4>

         ./blocking.sh