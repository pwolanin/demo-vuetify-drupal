# Requirements

You need node >= 18 and PHP >= 8.1

You'll also need npm and composer installed in advance.

For this example I set up a Vue application and Drupal and
connected them together.

# Vuetify 

I installed using npm:
https://github.com/vuetifyjs/create-vuetify

```
npm create vuetify
```

This is the official scaffolding tool for Vuetify.

# Drupal

I created a new codebase using composer:

```
composer create-project drupal/recommended-project mydrupal
```

This is the offical Drupal scaffold for new projects.


# How to Install the Demo

## First terminal: npm

To set up the demo, go into the project and install npm deps:

```
cd demo-vuetify-drupal
npm ci
```

## Second Terminal: composer + Drupal

Open a second terminal tab and go into the Drupal subdir
and do a composer install:

```
 cd demo-vuetify-drupal/mydrupal/
 composer install
```

If that went well, install Drupal with drush from the web dir:

```
cd web
../vendor/bin/drush site:install --account-pass=demo mydrupal
```

Now you can run a php local webserver:

```
php -S localhost:8888 .ht.router.php
```

You can log in to Drupal at  http://localhost:8888


## First terminal: npm

Go back to the first terminal and start the dev server:

```
npm run dev
```

Navigate to the page:
http://localhost:3000/
