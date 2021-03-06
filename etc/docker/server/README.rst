Rucio Server
============

Data Management for science in the Big Data era.

Rucio is a project that provides services and associated libraries for allowing scientific collaborations to manage large volumes of data spread across facilities at multiple institutions and organizations. Rucio has been developed by the [ATLAS](<https://atlas.cern/>). experiment. It offers advanced features, is highly scalable and modular. Rucio is a data management solution that could cover the needs of different communities in the scientific domain (e.g., HEP, astronomy, biology).

Documentation
-------------

General information and latest documentation about Rucio can be found at [readthedocs](<http://rucio.readthedocs.io>).

Developers
----------

For information on how to contribute to Rucio, please refer and follow our [CONTRIBUTING](<https://github.com/rucio/rucio/blob/master/CONTRIBUTING.rst>) guidelines.

Getting Started
---------------

This image provides the Rucio server both with and without SSL. It supports MySQL, PostgreSQL, Oracle and SQLite as database backends.

A simple server without SSL can be started like this:

```docker run --name=rucio-server -v /tmp/rucio.cfg:/opt/rucio/etc/rucio.cfg -p 80:80 -d rucio/rucio-server```

The rucio.cfg is used to configure the database backend.

If you want to enable SSL you would need to set the `RUCIO_ENABLE_SSL` variable and also need to include the host certificate, key and the the CA certificate as volumes. E.g.,:

```docker run --name=rucio-server -v /tmp/ca.pem:/etc/grid-security/ca.pem -v /tmp/hostcert.pem:/etc/grid-security/hostcert.pem -v /tmp/hostkey.pem:/etc/grid-security/hostkey.pem -v /tmp/rucio.cfg:/opt/rucio/etc/rucio.cfg -p 443:443 -e RUCIO_ENABLE_SSL=True -d rucio/rucio-server```

By default the output of the Apache web server is written directly to stdout and stderr. If you would rather direct them into separate files it can be done using the `RUCIO_ENABLE_LOGS` variable. The storage folder of the logs can be used as a volume:

```docker run --name=rucio-server -v /tmp/rucio.cfg:/opt/rucio/etc/rucio.cfg -v /tmp/logs:/var/log/httpd -p 80:80 -e RUCIO_ENABLE_LOGFILE=True -d rucio/rucio-server```

Environment Variables
---------------------

As shown in the examples above the rucio-server image can be configured using environment variables that are passed with `docker run`. Below is a list of all available variables and their behaviour:

`RUCIO_ENABLE_SSL`
==================
By default the rucio server runs without SSL on port 80. If you want to enable SSL set this variable to `True`. If you enable SSL you will also have to provide the host certificate and key and the certificate authority file. The server will look for `hostcert.pem`, `hostkey.pem` and `ca.pem` under `/etc/grid-security` so you will have to mount them as volumes. Furthermore you will also have to expose port 443.

`RUCIO_CA_PATH`
===============
If you are using SSL and want use `SSLCACertificatePath` and `SSLCARevocationPath` you can do so by specifying the path in this variable.

`RUCIO_DEFINE_ALIASES`
======================
By default the web server is configured with all common rest endpoints except the authentication endpoint. If you want to specify your own set of aliases you can set this variable to `True`. The web server then expects an alias file under `/opt/rucio/etc/aliases.conf`

`RUCIO_ENABLE_LOGFILE`
======================
By default the log output of the web server is written to stdout and stderr. If you set this variable to `True` the output will be written to `access_log` and `error_log` under `/var/log/httpd`.

`RUCIO_LOG_LEVEL`
=================
The default log level is `info`. You can change it using this variable.

`RUCIO_LOG_FORMAT`
=================
The default rucio log format is `%h\t%t\t%{X-Rucio-Forwarded-For}i\t%T\t%D\t\"%{X-Rucio-Auth-Token}i\"\t%{X-Rucio-RequestId}i\t%{X-Rucio-Client-Ref}i\t\"%r\"\t%>s\t%b`
You can set your own format using this variable.

`RUCIO_HOSTNAME`
================
This variable sets the server name in the apache config.

`RUCIO_SERVER_ADMIN`
====================
This variable sets the server admin in the apache config.

Getting Support
----------------

If you are looking for support, please contact our mailing list rucio-users@googlegroups.com
or join us on our [slack support](<https://rucio.slack.com/messages/#support>) channel.
