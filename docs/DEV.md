# LgHS Website - Développement

## Prérequis

Pour pouvoir développer, vous devez [avoir **Node.js** installé sur votre machine](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). Cela **afin de mettre à jour le CSS de Tailwind** lors de votre développement.

Étant donné qu'un serveur Apache est utilisé, sous Windows, vous devez également disposer de **[XAMPP](https://www.apachefriends.org/download.html) pour pouvoir tester** aisément l'application lors du développement de manière cohérente avec le [`.htaccess`](../.htaccess) à la racine du projet.

**Si vous ne disposez pas d'un certificat SSL** valide pour le développement, veuillez **commenter le bloc "FORCE HTTPS"** du fichier [`.htaccess`](../.htaccess) **le temps du développement**. Pour le déploiement **en production**, il faut bien sûr **le décommenter** pour forcer HTTPS.

## Développement

### XAMPP

Pour le développement, **vous pouvez avoir le dépôt cloné ailleurs que dans le dossier par défaut de XAMPP**. Il faut alors **configurer un "hôte virtuel"** en modifiant `C:\xampp\apache\conf\extra\httpd-vhosts.conf`.

Vous pouvez alors **ajouter un hôte virtuel en copiant-collant cela** et en **remplaçant le `DocumentRoot` et `Directory`** par votre propre chemin complet du dépôt cloné :

```conf
<VirtualHost *:80>
    DocumentRoot "C:/VOTRE_PATH_VERS_DEPOT"
    ServerName localhost

    <Directory "C:/VOTRE_PATH_VERS_DEPOT">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

**Démarrez alors Apache dans l'interface de XAMPP** et vous devriez pouvoir accéder à votre site sous : `http://localhost` (*ou `https` si vous disposez d'un certificat SSL*).

Pour plus d'infos sur les hôtes virtuels de XAMPP, veuillez consulter la documentation officielle consultable avec votre serveur Apache démarré sous le lien suivant : <https://localhost/dashboard/docs/configure-vhosts.html>

### Tailwind

Durant le développement, **vous allez sûrement manipuler des classes Tailwind**. Pour que le fichier CSS généré par Tailwind prenne en compte vos modifications, vous devez exécuter quelques commandes.

#### Premier lancement

**La première fois**, vous devez **télécharger les dépendances** requises par le site Web avec la commande suivante à la racine du projet :

```pwsh
npm install
```

Les dépendances **définies dans les fichiers `package-lock.json` et `package.json`** seront alors disponibles pour vos prochains lancements de l'application.

#### Actualisation automatique du CSS pendant le développement

Durant le développement, Tailwind peut **actualiser automatiquement** le contenu de son fichier CSS **selon les classes CSS que vous utilisez** au travers de votre site Web.

Dans notre cas, à la racine du projet, exécutez la commande suivante :

```pwsh
npx @tailwindcss/cli -i .\css\lghs-base.css -o .\css\lghs-full.css --watch
```

**Le fichier `css/lghs-full.css` sera alors automatiquement généré et/ou actualisé** selon vos modifications durant l'exécution de l'application dans la phase de développement.

#### Mise en production

Lorsque votre développement est terminé, pour pouvoir déployer proprement votre fichier CSS de Tailwind en production, vous pouvez :

1. **Cesser l'exécution** de l'application.
2. **Exécuter la commande** suivante à la racine du projet :

    ```pwsh
    npx @tailwindcss/cli -i .\css\lghs-base.css -o .\css\lghs-full.css --minify
    ```

Cela va alors **générer une version minifiée du CSS de Tailwind** dédié à votre application pour pouvoir déployer uniquement ce dont vous avez besoin.
