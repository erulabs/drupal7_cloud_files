Installation

- Download and install the Libraries (7.x-2.x branch) module
  http://drupal.org/project/libraries

- Install Composer:
  1. main: https://getcomposer.org/
  2. via chef: https://github.com/Morphodo/chef-composer

- Install the php-opencloud library (http://php-opencloud.com/) using Composer
  into sites/all/libraries/php-opencloud/:
  1. ```mkdir DOCROOT/sites/all/libraries/php-opencloud```
  2. ```cd DOCROOT/sites/all/libraries/php-opencloud```
  3. ```composer require rackspace/php-opencloud```

- Create a "php-opencloud.libraries.info" file in "DOCROOT/sites/all/libraries" with the following content:
```name = php-opencloud
machine name = php-opencloud
description = Rackspace SDK for OpenStack APIs
version = 1.12.1
files[php][] = vendor/autoload.php```

- Clone this repo into "DOCROOT/sites/all/modules/cloud_files"

- If you're NOT using Rackspace Cloud, or your targetting a Container OUTSIDE of your Region, you'll need to disable the use of ServiceNet, which my fork enables by default. You must remove the 'internalUrl' string from the calls to the Storage Service. Those references can be found here:
```$ fgrep -R 'internalUrl' *
cloud_files.module:      $object_store_service = $client->objectStoreService('cloudFiles', $rackspace_cloud_region, 'internalUrl');
cloud_files.module:        $object_store_service = $client->objectStoreService('cloudFiles', $region, 'internalUrl');
rackspacecloudfiles_streams.inc:    $object_store_service = $client->objectStoreService('cloudFiles', variable_get('rackspace_cloud_region'), 'internalUrl');
```
Remove the 3rd argument 'internalUrl', entirely. Better yet - move to Rackspace Cloud and use ServiceNet :P

- Enable the "Libraries" module, then the Cloud Files module.

- Configure the Cloud Files module

- Visit admin/config/media/file-system and set the Default download method to
  Rackspace Cloud Files.

- Depending on your configuration, you may need to use the CDN module to force URLs to the Rackspace Cloud.

- Add a field of type File or Image etc and set the Upload destination to Rackspace Cloud Files in the Field Settings tab.
