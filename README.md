Heroku Buildpack for Apple Transporter
======================================

Since installing the Transporter requires access to Apple's partner program,
and accepting the license, we didn't want to redistribute the package.

Instead it expects the package to be available for download at the URL
specified by `HEROKU_BUILDPACK_TRANSPORTER_DOWNLOAD_URL`.

A JVM runtime is required, i.e.
https://github.com/heroku/heroku-buildpack-jvm-common

Instructions
------------

  * https://itunespartner.apple.com/en/movies/faq/Transporter_Getting%20Set%20Up
  * Download the linux version
  * Install the linux version on a linux machine, read and accept the license
    terms
  * Transporter will be installed to /usr/local/itms
  * Package your installed transporter:

    cd /usr/local
    tar -czvf iTMSTransporter_linux_<version>.tar.gz ./itms

  * Upload your package to S3

    aws s3 cp iTMSTransporter_linux_<version>.tar.gz s3://<bucket>/iTMSTransporter_linux_<version>.tar.gz

  * Generate a signed URL for yourself, you may want a long expiration time
    (default is 1 hour)

    aws s3 presign s3://<bucket>/iTMSTransporter_linux_<version>.tar.gz

  * Set the URL on Heroku so the buildpack can access it

    heroku config:set HEROKU_BUILDPACK_TRANSPORTER_DOWNLOAD_URL='<presigned URL>'

  * Make sure the JVM is available in your Heroku build
    * https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app
    * https://github.com/heroku/heroku-buildpack-jvm-common
