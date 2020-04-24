# S3 Secure Downloads plugin for Craft CMS

This plugin will return a [signed URL](http://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html) used to allow temporary access to private objects with an expiring URL. You can optionally allow file downloads only for logged in users and force file downloads (useful for PDF files).

From the original developer, [Jonathan Melville](https://github.com/jonathanmelville/s3securedownloads):

> This plugin was originally developed for a client in the financial services industry who wanted to make sure only logged in users had access to downloads of financial reports, and download links couldn’t be shared. … Now you can keep your S3 objects private but grant temporary access to them with an expiring link. 

![Screenshot of the plugin settings.](./src/resources/screenshots/screenshot.png)

S3 Secure Downloads is built for Craft v3.x. For a version that runs on Craft v2.5.x, see [the original plugin](https://github.com/jonathanmelville/s3securedownloads).

## Installation

```sh
# Require the plugin with composer
composer require kennethormandy/craft-s3securedownloads

# Install the plugin via the Control Panel, or by running:
./craft install/plugin s3securedownloads
```

## Usage

Pass in an asset's entry id and it will return a signed URL for that asset:

```twig
{% set asset = entry.myAssetField.one() %}
<a href="{{ getSignedUrl(asset.id) }}">{{ asset }}</a>
```

By default, only users logged in will be able to generate the pre-signed URL. This can be changed within the plugin settings.

The generated a pre-signed AWS S3 URL will expire after 24 hours, or however long you have configured in the plugin settings.

## Events

- `kennethormandy\s3securedownloads\services\SignUrl`
  - `SignUrl::EVENT_BEFORE_SIGN_URL`
  - `SignUrl::EVENT_AFTER_SIGN_URL`

```php
use Craft;
use yii\base\Event;
use kennethormandy\s3securedownloads\events\SignUrlEvent;
use kennethormandy\s3securedownloads\services\SignUrl;

// …

Event::on(
    SignUrl::class,
    SignUrl::EVENT_BEFORE_SIGN_URL,
    function (SignUrlEvent $event) {
        $asset = $event->asset;
        Craft::info("Handle EVENT_BEFORE_SIGN_URL event here", __METHOD__);
    }
);

Event::on(
    SignUrl::class,
    SignUrl::EVENT_AFTER_SIGN_URL,
    function (SignUrlEvent $event) {
        $asset = $event->asset;
        Craft::info("Handle EVENT_AFTER_SIGN_URL event here", __METHOD__);
    }
);
```

## License

[The MIT License (MIT)](./LICENSE.md)

Copyright © 2016–2019 [Jonathan Melville](https://github.com/jonathanmelville/s3securedownloads)<br/>
Copyright © 2019–2020 [Kenneth Ormandy Inc.](https://kennethormandy.com)
