# Drupal 8 and React

## Lando
The backend runs on a [lando](https://docs.devwithlando.io/tutorials/drupal8.html) environment with with PHP 7.1.

Run the following command to start the lando environment:
```
cd backend
lando start
```
You should now be able to connect to the container on [https://d8-react.lndo.site](https://d8-react.lndo.site).

If you're starting from scratch there will be a "File not found"-error, as the backend is not installed yet (see next
section).

## Installation instructions - backend
The backend (Drupal) part is based on the drupal-composer project, so that
[most of its' documentation](https://github.com/drupal-composer/drupal-project) should apply here too.

Run following commands to get files needed for your Drupal installation:
```
cd backend
lando composer install
lando composer drupal-scaffold
```
Install Drupal with the predefined configuration:
```
cd backend/web
lando drush si config_installer --db-url=mysql://drupal8:drupal8@database/drupal8 --account-pass=admin --yes
```
You should now be able to see a fully installed Drupal 8 at [https://d8-react.lndo.site](https://d8-react.lndo.site).

## Installation instructions - frontend
The frontend (React) part is based on the create-react-app project, so that
[most of its' documentation](https://github.com/facebookincubator/create-react-app) should apply here too.

Run following commands to download the dependencies and start the development server:
```
cd frontend
npm install
npm start
```
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

## Connect the app with the backend
Drupal is configured to not allow sending content to a different domain than its own. Following needs to be done:

Copy the "default.services.yml" as "services.yml":
```
cp backend/web/sites/default/default.services.yml backend/web/sites/default/services.yml 
```
and change following values:
```
  cors.config:
    enabled: true
    # Specify allowed headers, like 'x-allowed-header'.
    allowedHeaders: ['Content-Type', 'Access-Control-Allow-Origin']
    # Specify allowed request methods, specify ['*'] to allow all possible ones.
    allowedMethods: ['*']
```
Clear the cache, so that the changes take effect.
```
lando drupal cr all
```
Note, there is not authentication in the app (yet), so anonymous user in Drupal are assigned the permission "Execute
arbitrary GraphQL requests".
