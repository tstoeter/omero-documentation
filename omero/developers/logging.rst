Omero logging
=============

All OMERO components written in Java use the
`SLF4J <https://www.slf4j.org>`_ logging facade, typically backed
by `Logback <https://logback.qos.ch/>`_; all
components written in python use the built-in ``logging`` module.

.. Warning::  
    Refrain from calling ``logging.basicConfig()`` anywhere in your
    module except in ``if __name__ == "__main__"`` blocks.

Java clients
------------

Java clients log to ``$HOME/omero/log``. The number of files and their
size are limited.

:source:`logback-cli.xml<etc/logback-cli.xml>`
controls the output for the command line importer: all logging goes
to standard err, while useful output (pixel ids, or used files) goes to
standard out. It is contained within the ``omero-blitz.jar`` itself. Therefore, to
modify the settings use `-Dlogback.configurationFile=/path/to/logback.xml` or
similar.

OMERO.insight logging is configured via `logback.xml`
which is available in the config/ directory of any OMERO.insight install.

Java servers
------------

Java server components are configured by passing
``-Dlogback.configurationFile=etc/logback.xml`` to each Java process.
:source:`logback.xml <etc/logback.xml>` includes the ``scan`` attribute so
that changes to the logging configuration are `automatically reloaded at
regular intervals
<https://logback.qos.ch/manual/configuration.html#autoScan>`_.

By default, the output from logback is sent to a file named :file:`<servername>.log`
under the directory specified by :property:`omero.logging.directory`, by
default :file:`var/log`.
Once files reach a size specified by :property:`omero.logging.logsize`, 500MB
by default, they are rolled over to :file:`<servername>.log.1`,
:file:`<servername>.log.2`, etc. The maximum of rotated log files is specified
by :property:`omero.logging.lognum`.
Once the files have rolled over, you can safely delete or compress (bzip2,
gzip, zip) them. Alternatively, once you are comfortable with the
stability of your server, you can either reduce logging or the number
and size of the files kept. **Note:** if something goes wrong with your
server installation, the log files can be very useful in tracking down
issues.

In addition, each import process logs to a file under the managed
repository which matches the timestamped fileset directory's name.
For example, if an imported fileset is uploaded to
:file:`/OMERO/ManagedRepository/userA_1/2013-06/17/12-00-00.000`, then
the log file can be found under
:file:`/OMERO/ManagedRepository/userA_1/2013-06/17/12-00-00.000.log`.

Python servers
--------------

Python servers are configured by a call to
``omero.util.configure_server_logging(props)``. The property values are
taken from the configuration file passed to the server via icegridnode.
For example, the config file for Processor-0 can be found in
``var/master/servers/Processor-0/config/config``. These values come from
:file:`etc/grid/templates.xml`.

Similar to the Java servers, logging is configured to be written to
:file:`var/log/<servername>.log` and to maintain 9 backups of at most 500MB
by default. All the ``omero.logging`` properties can be specified via configuration - 
see :ref:`log_configuration` for more details

stdout and stderr
-----------------

Though all components try to avoid it, some output will still go to
stdout/stderr. On non-Windows systems, all of this output will be sent
to the :file:`var/log/master.out` and :file:`var/log/master.err` files.
