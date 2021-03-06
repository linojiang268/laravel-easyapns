A Laravel package of Easy APNS

* This is a very early beta. Use at your own risk. Please report any bugs *

- **Author:** Keith Slater
- **Website:** [http://www.keithslater.com](http://www.keithslater.com)

Code ported from [Easy APNS](http://www.easyapns.com/)

## Installation Instructions

Add the following to your composer.json file

```json
"keithslater/easyapns": "dev-master"
```

Then run composer update

After the project is updated, add the following to your app.php file:

```php
'providers' => array(
    'Keithslater\Easyapns\EasyapnsServiceProvider',
);
```

The following command runs the migration to your database

    $ php artisan migrate --package="keithslater/easyapns"

This command copies the config file to app/config/packages/keithslater/easyapns/config.php

    $ php artisan config:publish keithslater/easyapns

Upload your development and production .pem files to app/config/packages/keithslater/easyapns/. If you need to convert your certificates to pem certificate files, I recommend following [this answer](http://stackoverflow.com/questions/1762555/creating-pem-file-for-apns) on Stackoverflow.

Modify app/config/packages/keithslater/easyapns/config.php as needed

## APNS command

Finds messages in the database that are still queued and will push them. Sends only first message for device

    $ php artisan apns fetch

Similar to fetch but sends all messages for each device

    $ php artisan apns flush

You might want to set one of these commands as a cronjob

## Usage

Add the following to the header where you are calling APNS

```php
use \Keithslater\Easyapns\Easyapns;
```

Then you will be able to use Easy APNS like normal:

```php
$apns = new Easyapns();
$apns->newMessage(1);
$apns->addMessageAlert('Test message sent');
$apns->queueMessage();
$apns->processQueue();
```

Refer to [Easy APNS](http://www.easyapns.com/) for more information.

### Responding to the Apple Delegate methods

I recommend setting up a route named something similar to apns. From that route call Easyapns with all input requests. Something similar to this example:

```php
Route::get('apns', function() {
        $apns = new Easyapns(Input::all());
});
```

These few lines of code will respond to the delegate method from your app and add the new device to the database. Just change the host and urlString strings in the app delegate methods to point here.