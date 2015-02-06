<h1>Bien commencer un projet web...</h1>

- Veiller à ce que <strong>Node.js</strong> (http://nodejs.org/download/) soit installé, 
- Veiller à ce que <strong>Bower</strong> (http://bower.io/) soit installé, 
- Veiller à ce que <strong>GIT</strong> (http://git-scm.com/book/fr/v1/D%C3%A9marrage-rapide-Installation-de-Git) soit installé
- Veiller à ce que le répertoire <kbd>/Projects</kbd> contienne un fichier .bowerrc avec le contenu suivant :
```{
  "directory": "/"
}```
(Cela permettra d'installer Bootstrap à la racine du répertoire Projects.)
- Veiller à ce que le projet soit accessible via une <strong>url locale</strong>  (exemple : monprojetweb.local.fr)(http://www.dewep.net/Blog/Article-6/Creer-un-nom-de-domaine-en-local-avec-Apache-sur-Windows). Cela permettra la consultation du site local sur plusieurs supports.

<h2>Step 1 : Bower</h2>
- Se déplacer à la racine du répertoire <kbd>/Projects</kbd>
- Lancer la commande <kbd>bower install bootstrap</kbd> 
- Renommer le réperdoire <kbd>/Bootstrap</kbd> avec le nom du projet concerné, exemple <kbd>projet</kbd>

<h2>Step 2</h2>
- Lancer la commande <kbd>sudo npm install -g grunt-cli</kbd>
- Se déplacer à la racine du répertoire <kbd>/Projet</kbd>
- Lancer la commande <kbd>npm install</kbd>

<h2>Step 3</h2>
- Lancer la commande <kbd>git init</kbd>
- Lancer la commande <kbd>echo node_modules>.gitignore</kbd> pour créer un fichier .gitignore et permettre à GIT d'ignorer les modules NODE.js
- Lancer la commande <kbd>git add</kbd> puis <kbd>git commit -m "Initialisation"</kbd> puiq <kbd>git status</kbd> pour contrôler que tout est en ordre.

<h2>Step 4</h2> 
- Remplacer le contenu du fichier <kbd>Gruntfile.js</kbd> (dans le répertoire du projet concerné) par :

```
module.exports = function(grunt) {

  //Initializing the configuration object
    grunt.initConfig({

      // Task configuration
    less: {
        development: {
            options: {
              compress: true,  //minifying the result
            },
            files: {
              //compiling frontend.less into frontend.css
              "dist/css/main.min.css":"less/bootstrap.less"
            }
        }
    },
    concat: {
      options: {
        separator: ';',
      },
      js_frontend: {
        src: [
          '../jquery/dist/js/jquery.js',/Attention à avoir le répertoire jquery à la racine du répertoire Projects. Téléchargé à chaque bower install
          'js/transition.js',
          'js/alert.js',
          'js/button.js',
          'js/carousel.js',
          'js/collapse.js',
          'js/dropdown.js',
          'js/modal.js',
          'js/tooltip.js',
          'js/popover.js',
          'js/scrollspy.js',
          'js/tab.js',
          'js/affix.js',
          'js/configuration.js'
        ],
        dest: 'dist/js/main.js',
      },
    },
    uglify: {
      options: {
        mangle: false  // Use if you want the names of your functions and variables unchanged
      },
      frontend: {
        files: {
          'dist/js/main.min.js': 'dist/js/main.js',
        }
      },
    },
    watch: {
        js_frontend: {
          files: [
            //watched files
            '../jquery/dist/jquery.js',//Attention à avoir le répertoire jquery à la racine du répertoire Projects. Téléchargé à chaque bower install 
            'js/transition.js',
            'js/alert.js',
            'js/button.js',
            'js/carousel.js',
            'js/collapse.js',
            'js/dropdown.js',
            'js/modal.js',
            'js/tooltip.js',
            'js/popover.js',
            'js/scrollspy.js',
            'js/tab.js',
            'js/affix.js',
            'js/configuration.js'
            ],   
          tasks: ['concat:js_frontend','uglify:frontend'],     //tasks to run
          options: {
            livereload: true                        //reloads the browser
          }
        },
        less: {
          files: ['less/*.less'],  //watched files
          tasks: ['less'],                          //tasks to run
          options: {
            livereload: true                        //reloads the browser
          }
        },
        html: {
          files: ['*.php'],
          options: {
              livereload: true
          }
        }
      }
    });

  // Plugin loading
  grunt.loadNpmTasks('grunt-contrib-concat');
  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-contrib-less');
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Task definition
  grunt.registerTask('default', ['watch']);

};
```

<strong>IMPORTANT : en cas de rajout de fichier JS, les placer dans le répertoire <kbd>/js</kbd> et les déclarer dans le fichier <kbd>Gruntfile.js</kbd></strong>

<h2>Step 5</h2> 
- Créer un fichier <kbd>configuration.js</kbd> dans le répertoire <kbd>/js</kbd>. Ce fichier intégrera le JS personnalisé.
- Créer un fichier <kbd>style.less</kbd> dans le répertoire <kbd>/less</kbd>. Ce fichier intégrera le CSS personnalisé. Ne pas oublier de le déclarer dans le fichier <kbd>bootstrap.less</kbd>. 

<h2>Step 6 </h2>
- Lancer la commande <kbd>grunt watch</kbd> et ouvrir le projet sur tous les navigateurs et supports. A chaque <kbd>CTRL + S</kbd> dans un fichier LESS, JS ou HTML, tous les instances seront rafraîchies automatiquement.
<<strong>IMPORTANT : Le live reload peut ne pas fonctionner la première fois. Il faut simplement sélectionner un fichier less, html et js et faire un CTRL + S pour chacun et rafraîchir la page.</strong>






<h1>Bien continuer un projet web...</h1>

- Ne jamais modifier les fichiers sources Bootstrap au risque de ne pas pouvoir faire de mise à jour lors des nouvelles releases.
- Déterminer les jalons du projet afin de commiter aux bons moments.
- Déterminer une méthode de nommage des classes en début de projet.
- Déterminer toutes les variables LESS (couleurs, polices, etc) en début de projet.
- Analyser les besoins fonctionnels des maquettes et en déduire les variantes et/ou implications.
- Analyser les maquettes afin de déterminer les zones récurentes. 
- Analyser les contenus et déterminer à l'avance les balises à utiliser.
- Penser à l'implémentation des futurs développement (maximiser les structures sémantiques sans id par exemple).

<strong>Mais aussi :</strong>

- Intégrer les contenus avec des balises correspondant à leur nature (exemple : <kbd><abbr><dfn></kbd>)
- Exécuter du code JS directement en console après le chargement du DOM pour débugger.
- Indenter le code en space 4
- Intégrer également le CSS/LESS avec Emmet
- Remplir les attributs alt sur les images et title sur les liens.

<h1>Bien finir un projet</h1>

<h2>Step 1 : Google</h2>

- Créer un compte Google Analytics puis récupérer et intégrer le code de suivi dans le footer.
- Générer un fichier <kbd>sitemap.xml</kbd> et le déclarer les Webmasters Tools.
- Générer un fichier <kbd>robots.txt</kbd> et le déclarer les Webmasters Tools.
- Tester chaque page sur Google Speed Insights et apporter les corrections nécessaires en fonction des résultats.

<h2>Step 2 : W3C</h2>

- Soumettre chaque page du site au W3C et ajuster le code en focntion des erreurs.

<h2>Step 3 : HTACCESS</h2>

- Générer un fichier .htacess incluant le contenu suivant :

```
<IfModule mod_deflate.c>
    # MOD_DEFLATE COMPRESSION
    SetOutputFilter DEFLATE
    AddOutputFilterByType DEFLATE text/html text/css text/plain text/xml application/x-javascript application/x-httpd-php
    #Pour les navigateurs incompatibles
    BrowserMatch ^Mozilla/4 gzip-only-text/html
    BrowserMatch ^Mozilla/4\.0[678] no-gzip
    BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
    BrowserMatch \bMSI[E] !no-gzip !gzip-only-text/html
    #ne pas mettre en cache si ces fichiers le sont déjà
    SetEnvIfNoCase Request_URI \.(?:gif|jpe?g|png)$ no-gzip
    #les proxies doivent donner le bon contenu
    Header append Vary User-Agent env=!dont-vary
</IfModule>

<IfModule mod_rewrite.c>
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
    RewriteCond %{REQUEST_URI} !^/(media|skin|js)/
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-l
    RewriteRule .* index.php [L]
</IfModule>

ErrorDocument 404 http://www.fredericblancheton.fr/404.php
Options -Indexes
```

- Les modules permettent d'optimiser les performances Front End du site.
- La dernière ligne permet d'interdire l'accès au répertoires source.
