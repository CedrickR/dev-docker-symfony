# Symfony
## Table of Contents <a id="contents"></a>

- [Envoyer des emails](#Mails)
- [Installer Bootstrap](#bootstrap)
  * [Installation des dépendances](#dependances)
  * [Fichier app/config/config.yml](#config_yml)
  * [Activer le bundle assets](#bundle_assets)
- [Copies des assets](#assets)


## Envoyer des mails <a id="mails"></a> 


1. Dans le fichier app/config/parameters.yml

	```
		parameters:
		    mailer_transport: mail
		    mailer_host: 127.0.0.1
		    mailer_port: 1025
		    mailer_user: null
		    mailer_password: null
	```
	
2. Dans le fichier app/config/config_dev.yml

	```
	swiftmailer:
		    delivery_addresses: ['me@symfony.dev']
	```
<p style='text-align:right'>[Table des matières](#contents)

## Installer Bootstrap <a id="bootstrap"></a>

Avant de commencer l'ajout de bundles, bien penser à fermer les fichiers config.yml et parameters.yml 

### Installation des dépendances <a id="dependances"></a>

	```
	        "twbs/bootstrap" : "3.*",
	        "components/jquery": "dev-master",
	        "oyejorge/less.php": "dev-master"
	        
	        composer require symfony/assetic-bundle
	```

### Fichier app/config/config.yml <a id="config_yml"></a>

	```
	# Assetic Configuration
	assetic:
	   debug:          "%kernel.debug%"
	   use_controller: false
	   bundles:    [ ]
	   java: /usr/bin/java
	   #java: C:\Program Files\Java\jdk1.8.0_65\bin\java.exe
	   filters:
	       cssrewrite: ~
	       cssembed:
	           jar: "%kernel.root_dir%/Resources/java/cssembed.jar"        
	       yui_js:
	           jar: "%kernel.root_dir%/Resources/java/yuicompressor.jar"
	       yui_css:
	           jar: "%kernel.root_dir%/Resources/java/yuicompressor.jar"
	
	       lessphp:
	           file: "%kernel.root_dir%/../vendor/oyejorge/less.php/lessc.inc.php"   
	           apply_to: ".less$"
	       
	   
	   
	   assets:
	   
	       jquery_js:
	           inputs:
	               - "%kernel.root_dir%/../vendor/components/jquery/jquery.min.js"            
	           filters: [?yui_js]
	           output: js/jquery.min.js
	           
	       bootstrap_css:
	           inputs:
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/less/bootstrap.less"
	           filters:
	               - lessphp
	               - cssrewrite
	           output: css/bootstrap.css            
	
	       bootstrap_js:
	           inputs:
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/affix.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/alert.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/button.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/carousel.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/collapse.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/dropdown.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/modal.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/tooltip.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/popover.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/scrollspy.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/tab.js"
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/js/transition.js"
	           filters: [?yui_js]
	           output: js/bootstrap.js             
	
	       
	       fonts_glyphicons_eot:
	           inputs:
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/fonts/glyphicons-halflings-regular.eot"
	           output: "fonts/glyphicons-halflings-regular.eot"
	       fonts_glyphicons_svg:
	           inputs:
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/fonts/glyphicons-halflings-regular.svg"
	           output: "fonts/glyphicons-halflings-regular.svg"
	       fonts_glyphicons_ttf:
	           inputs:
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/fonts/glyphicons-halflings-regular.ttf"
	           output: "fonts/glyphicons-halflings-regular.ttf"
	       fonts_glyphicons_woff:
	           inputs:
	               - "%kernel.root_dir%/../vendor/twbs/bootstrap/fonts/glyphicons-halflings-regular.woff"
	           output: "fonts/glyphicons-halflings-regular.woff"
	        theme_css:
           inputs:
               - "%kernel.root_dir%/Resources/css/theme.css"
           output: css/theme.css 
	```

### Activer le bundle assets <a id="bundle_assets"></a>
	
	```php
	// app/AppKernel.php
	// ...
	class AppKernel extends Kernel
	{
	    // ...
	
	    public function registerBundles()
	    {
	        $bundles = array(
	            // ...
	            new Symfony\Bundle\AsseticBundle\AsseticBundle(),
	        );
	
	        // ...
	    }
	}
	```
### Ajouter bootstrap dans les fichiers html <a id="bootstrap_html"></a>
* Dans le fichier base.html.twig
		
	```
	<!DOCTYPE html>
	<html lang="fr">
	    <head>
	        <meta charset="utf-8">
	        <meta http-equiv="X-UA-Compatible" content="IE=edge">
	        <meta name="viewport" content="width=device-width, initial-scale=1">
	        <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
	        <meta name="description" content="">
	        <meta name="author" content="">
	       <title>{% block title %}Mon titre{% endblock %}</title>
	            {% block stylesheets %}
	                <link rel="stylesheet" href="{{ asset('css/bootstrap.css') }}">
	                <link rel="stylesheet" href="{{ asset('css/theme.css') }}">
	                <script src="{{ asset('js/jquery.min.js') }}"></script>
	                <script src="{{ asset('js/bootstrap.js') }}"></script>
	                <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
	                <!--[if lt IE 9]>
	                <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
	                <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
	                <![endif]-->
	            {% endblock %}
	       <link rel="icon" type="image/x-icon" href="{{ asset('favicon.ico') }}" />
	    </head>
	    <body>
	        {% block nav %}
	            <!-- Fixed navbar -->
	            <nav class="navbar navbar-inverse navbar-fixed-top">
	            <div class="container">
	                <div class="navbar-header">
	                <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
	                    <span class="sr-only">Toggle navigation</span>
	                    <span class="icon-bar"></span>
	                    <span class="icon-bar"></span>
	                    <span class="icon-bar"></span>
	                </button>
	                <a class="navbar-brand" href="#">Bootstrap theme</a>
	                </div>
	                <div id="navbar" class="navbar-collapse collapse">
	                <ul class="nav navbar-nav">
	                    <li class="active"><a href="#">Home</a></li>
	                    <li><a href="#about">About</a></li>
	                    <li><a href="#contact">Contact</a></li>
	                    <li class="dropdown">
	                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
	                    <ul class="dropdown-menu">
	                        <li><a href="#">Action</a></li>
	                        <li><a href="#">Another action</a></li>
	                        <li><a href="#">Something else here</a></li>
	                        <li role="separator" class="divider"></li>
	                        <li class="dropdown-header">Nav header</li>
	                        <li><a href="#">Separated link</a></li>
	                        <li><a href="#">One more separated link</a></li>
	                    </ul>
	                    </li>
	                </ul>
	                </div><!--/.nav-collapse -->
	            </div>
	            </nav>
	
	        {% endblock %}
	        <div class="container theme-showcase" role="main">
	        {% block body %}{% endblock %}
	        </div> <!-- /container -->
	        {% block javascripts %} <script src="{{ asset('js/bootstrap.js') }}"></script>{% endblock %}
	    </body>
	</html>

* Dans le fichier index.html.twig

	```	
	{% extends 'base.html.twig' %}

	{% block body %}
	      <!-- Main jumbotron for a primary marketing message or call to action -->
	      <div class="jumbotron">
	        <h1>Theme example</h1>
	        <p>This is a template showcasing the optional theme stylesheet included in Bootstrap. Use it as a starting point to create something more unique by building on or modifying it.</p>
	      </div>
	{% endblock %}
	```
* Ajout du fichier theme.css dans le dossier Resources/css/

	```
	body {
	  padding-top: 100px;
	  padding-bottom: 30px;
	}
	
	.theme-dropdown .dropdown-menu {
	  position: static;
	  display: block;
	  margin-bottom: 20px;
	}
	
	.theme-showcase > p > .btn {
	  margin: 5px 0;
	}
	
	.theme-showcase .navbar .container {
	  width: auto;
	}
	```

<p style='text-align:right'>[Table des matières](#contents)

## Copies des assets <a id="assets"></a>

On lance une copie des assets à l’aide de la commande

	 $ php app/console assetic:dump

