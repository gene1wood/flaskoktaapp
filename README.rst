Overview
========

Reference architecture for a Flask application, secured with Okta using SAML

Configuration
=============

The configuration file for the application is located in `/etc` and is named
`APPNAME.yaml` for a given application name.

secret_key
----------

A unique secret key to secure Flask sessions

metadata_url_for
----------------

A dictionary of all SAML identity providers with the name of the identity
provider as the key and the identity providers SAML metadata URL as the
value.

idp_name
--------

The name of the preferred SAML identity provider.

acs_url_scheme
--------------

Set this to `http` or `https` depending on how you're serving up the web UI.

PREFERRED_URL_SCHEME
--------------------

Set this to `http` or `https` depending on how you're serving up the web UI.

loglevel
--------

The `level <https://docs.python.org/2/library/logging.html#levels>`_ to set for logging.

Example Configuration
---------------------

Here is an example configuration for two foreign AWS accounts

::

    --- 
      secret_key: "11111111-1111-1111-1111-111111111111"
      idp_name: oktadev
      metadata_url_for: 
        okta: "http://idp.oktadev.com/metadata"
      PREFERRED_URL_SCHEME: https
      acs_url_scheme: https
      loglevel: DEBUG

