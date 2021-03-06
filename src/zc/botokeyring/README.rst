===============================
Integration of boto and keyring
===============================

Note: My hope is that this package will become redundant by
integrating something very similar into boto itself.

Boto is awesome for controlling AWS.  Unfortunately, there are times
when specifying credentials is awkward.  Typically, one has to either:

- includint the AWS password (aws_secret_access_key) in a clear text
  dot file, which is insecure, or

- setting an environment variable with the password, which is
  inconvenient and insecure on multi-user systems.

It would be better to use a secure keyring to store the password.
zc.botokeyring provides exactlly that capability. To use it, set
aws_access_key_id in the Credentials section of your boto
configuration filem as usual, and instead of setting
aws_secret_access_key, set keyring to the name of a keyring containing
the password::

  [Credentials]
  aws_access_key_id = 1234
  keyring = test

.. -> config

    >>> with open('.boto', 'w') as f:
    ...     f.write(config)

    >>> import boto, boto.provider, boto.pyami.config, zc.botokeyring
    >>> _ = reload(boto.pyami.config)
    >>> _ = reload(boto)
    >>> _ = reload(boto.provider)

    >>> zc.botokeyring.setup()

    >>> p = boto.provider.Provider('aws')
    >>> p.access_key
    '1234'
    >>> p.secret_key
    'test1234pw'

Changes
=======

0.1.0 (2012-12-02)
==================

Initial release
