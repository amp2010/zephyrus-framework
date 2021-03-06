<p align="center">
    <img align="center" src="https://cloud.githubusercontent.com/assets/4491532/21667795/e69dec6e-d2c9-11e6-8563-133291489ed3.png" width="45%">           
</p>

---
<p align="center"><i>Framework PHP simple et léger</i></p>

---

[![Maintainability](https://api.codeclimate.com/v1/badges/6981c700b82a43834672/maintainability)](https://codeclimate.com/github/dadajuice/zephyrus/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/6981c700b82a43834672/test_coverage)](https://codeclimate.com/github/dadajuice/zephyrus/test_coverage)
[![Travis](https://img.shields.io/travis/dadajuice/zephyrus.svg)]()
[![StyleCI](https://styleci.io/repos/77175312/shield?branch=master)](https://styleci.io/repos/77175312)
[![GitHub issues](https://img.shields.io/github/issues/dadajuice/zephyrus.svg)]()
[![GitHub release](https://img.shields.io/github/release/dadajuice/zephyrus.svg)]()
[![license](https://img.shields.io/github/license/dadajuice/zephyrus.svg)]()

# Fonctionnalités
* Structure de projet simpliste
* Routes avec mécanisme MVC
* Préprocesseur HTML _[Pug](https://github.com/pug-php/pug)_ intégré pour le rendu des vues
* Patron courtier intégré pour l’interaction avec une base de données
* Mécanisme uniforme pour gérer les validations d’un formulaire
* Mesures contre le détournement de session intégrées
* Données de la session chiffrées sur le serveur
* Protection automatique des formulaires contre les attaques CSRF
* Mécanisme d’autorisation sur les routes
* Détecteur d’intrusion intégré (_[Expose](https://github.com/enygma/expose)_)
* Manipulation simplifiée des en-têtes CSP
* Configuration d’un projet simple et rapide
* Plusieurs utilitaires rapides : cryptographie, pagination, validations, téléversements, gestionnaire d'erreurs, transport de messages, etc.

# Installation
Zephyrus nécessite PHP 7 et, présentement, supporte uniquement Apache comme serveur web (pour un autre type de serveur, il suffirait d’adapter les fichiers .htaccess). Le gestionnaire de dépendance [Composer](https://getcomposer.org/) est également requis. La structure résultante de l’installation contient plusieurs exemples pour faciliter les premiers pas.

#### Option 1 : Installation depuis composer (recommandé)
```
$ composer create-project zephyrus/framework nom_projet
```

#### Option 2 : Depuis une archive
```
$ mkdir nom_projet
$ cd nom_projet
$ wget https://github.com/dadajuice/zephyrus-framework/archive/v0.9.9.2.tar.gz
$ tar -xvf v0.9.9.2.tar.gz --strip 1
$ composer install
```

#### Option 3 : Depuis les sources (version de développement)
```
$ git clone https://github.com/dadajuice/zephyrus-framework.git
$ composer install  
```

# Utilisation

#### Exemple 1 : Affichage d'une page HTML simple

app/controllers/SampleController.php
```php
<?php namespace Controllers;

use Zephyrus\Application\Controller;

class SampleController extends Controller
{
    public function initializeRoutes()
    {
        $this->get("/sample", "index");
        $this->get("/sample/{id}", "read");
    }

    public function index()
    {
        $this->render('sample', ['message' => 'Bonjour le monde !']);
    }
    
    public function read($id)
    {
        $this->render('sample', ['message' => 'ID = ' . $id]);
    }
}  
```

app/views/sample.pug
```pug
doctype html
html
  head
    title Exemple
    meta(charset="utf-8")
  body
    #container
      h2=message
```

#### Exemple 2 : Traitement d'un formulaire en POST
app/controllers/SampleController.php
```php
<?php namespace Controllers;

use Zephyrus\Application\Controller;
use Zephyrus\Application\Flash;
use Zephyrus\Application\Form;
use Zephyrus\Utilities\Validator;

class SampleController extends Controller
{
    public function initializeRoutes()
    {
        $this->get("/sample", "index");
        $this->get("/sample/{id}", "read");
        $this->post("/sample", "insert");
    }

    public function index()
    {
        $this->render('sample', ['message' => 'Bonjour le monde !']);
    }

    public function read($id)
    {
        $this->render('sample', ['message' => 'ID = ' . $id]);
    }

    public function insert()
    {
        $form = $this->buildForm();       
        $form->addRule('firstname', Validator::NOT_EMPTY, "Le prénom ne doit pas être vide");
        $form->addRule('lastname', Validator::NOT_EMPTY, "Le nom ne doit pas être vide");
        $form->addRule('email', Validator::NOT_EMPTY, "Le courriel ne doit pas être vide");
        $form->addRule('email', Validator::EMAIL, "Le courriel est invalide", Form::TRIGGER_FIELD_NO_ERROR);

        if (!$form->verify()) {
            $messages = $form->getErrorMessages();
            Flash::error($messages);
            $this->response->redirect("/sample");
        }

        echo "Bravo !";
    }
}
```

app/views/sample.pug
```pug
doctype html
html
  head
    title Exemple
    meta(charset="utf-8")
  body
    #container
      h2=message
      if flash.error
        div.error
          ul
          each err in flash.error
            li #{err}
      form(method="post", action="/sample")
        input(type="text", name="firstname", placeholder="Prénom", value=val('firstname'))
        br
        input(type="text", name="lastname", placeholder="Nom", value=val('lastname'))
        br
        input(type="text", name="email", placeholder="Courriel", value=val('email'))
        br
        button(type="submit") Envoyer
```

# Contribution

#### Sécurité
Veuillez communiquer en privé pour tout problème pouvant affecter la sécurité des applications créées avec ce framework.

#### Bogues et fonctionnalités
Pour rapporter des bogues, demander l’ajout de nouvelles fonctionnalités ou faire des recommandations, n’hésitez pas à utiliser l’[outil de gestion des problèmes](https://github.com/dadajuice/zephyrus-framework/issues) de GitHub.

#### Développement
Vous pouvez contribuer au développement de Zephyrus en soumettant des [PRs](https://github.com/dadajuice/zephyrus-framework/pulls).

# License
MIT (c) David Tucker
