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


Installation
============

Create a new "app" in okta

- https://mozilla-admin.okta.com/admin/apps/active
- Add Application : https://mozilla-admin.okta.com/admin/apps/add-app
- Create New App
   - SAML 2.0
   - Create
- App Name : YourAppNameGoesHere
- Next
- Single sign on URL : https://example.security.mozilla.com/saml/sso/okta
- Audience URI (SP Entity ID) : https://example.security.mozilla.com/saml/sso/okta
- Attribute Statements (Optional)

  +-----------+-------------------+
  | Name      | Value             |
  +===========+===================+
  | FirstName | ${user.firstName} |
  +-----------+-------------------+
  | LastName  | ${user.lastName}  |
  +-----------+-------------------+
  | Email     | ${user.email}     |
  +-----------+-------------------+

- Next
- Are you a customer or partner? : I'm an Okta customer adding an internal app
   - App type : Check "This is an internal app that we have created"
   - Finish
- In the "Settings" page, copy the URL of the link titled "Identity Provider metadata"
- Paste the URL into either the /opt/puppet/hiera/ENVIRONMENT.yaml file as the `flaskoktaapp::saml_url` or into your credstash
- Assign users to the app
   - Click the "People" tab in the app screen in okta
   - Click "Assign to People"
   - Search for the user
   - Click "Assign"
   - Click "Save and go back"
   - Click "Done"
- If using credstash
   - Add to the CloudFormation template an IAM role granting access to the dynamodb
   - Create credstash stored secrets that are listed in `puppet/hiera/ENVIRONMENT.yaml`
   - Create KMS grants so the instance can decrypt the credentials
   - Enable installing puppet-credstash in the Puppetfile by uncommenting
   - Enable installing credstash in the cloudformation template's cloud-config by uncommenting
- If not
   - Comment out the credstash puppetmodule in `/opt/puppet/Puppetfile`
   - Add your secrets to `/opt/puppet/hiera/ENVIRONMENT.yaml`
- Deploy a CloudFormation stack using the template
- Create a DNS name pointing to the instance
- Create Certs (optional)
- Create EIP (optional)