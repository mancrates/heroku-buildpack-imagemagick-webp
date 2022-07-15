heroku-buildpack-imagemagick-webp
=================================

This is a [Heroku buildpack](https://devcenter.heroku.com/articles/buildpacks) for vendoring the [ImageMagick](https://www.imagemagick.org) binaries built with [webp](https://github.com/webmproject/libwebp) support into your project.

This buildpack will download both libraries, build them from source, and install them along side your app. Your `$PATH` and `$LD_LIBRARY_PATH` will also be updated to use this version of ImageMagick instead of the default Heroku one, which does _not_ currently have libwebp support.

Since this buildpack is building libwebp and ImageMagick from source your first deploy will take longer. However after building once the installed libraries are stored in a cache directory and will be used for all future deploys. If for any reason you want to force a rebuild of libwebp and ImageMagick you will need to [Clear the cache](#clear-cache).

## Install
In your project root:

`heroku buildpacks:add https://github.com/mancrates/heroku-buildpack-imagemagick  --index 1 --app HEROKU_APP_NAME`

"index 1" means that imagemagick will be installed first.

## Verify Installation
You can verify that ImageMagick was built with libwebp support by running the following:

`heroku run "identify -list format" --app HEROKU_APP_NAME`

If the output includes `WEBP* rw-   WebP Image Format (libwebp x.x.x)` then you're all set.

## Changing version
* Determine the new version you want to use.
  * You can visit the ImageMagick project repo to view [Releases](https://imagemagick.org/download/releases) and the [ChangeLog](https://github.com/ImageMagick/ImageMagick/releases) by using the github compare tags feature.
  * You can visit the Webp project repo to view [Releases](https://storage.googleapis.com/downloads.webmproject.org/releases/webp/index.html) and the [ChangeLog](https://chromium.googlesource.com/webm/libwebp/) for each tag
* Edit the `bin/compile` file and change out the version number.
* Clear cache, as shown below, and redeploy your app to Heroku.

## Clear the cache
Since the installation is cached you might want to clean it out due to config changes. Check out the [heroku-repo](https://github.com/heroku/heroku-repo) plugin.

1. `heroku plugins:install heroku-repo`
2. `heroku repo:purge_cache -app HEROKU_APP_NAME`
