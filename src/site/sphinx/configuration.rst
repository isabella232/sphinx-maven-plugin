.. _`Sphinx commandline documentation`: http://sphinx.pocoo.org/man/sphinx-build.html?highlight=command%20line
.. _`Sphinx tag documentation`: http://sphinx.pocoo.org/markup/misc.html#tags
.. _`Jython`: http://www.jython.org/

Configuration
=============

The ``sphinx-maven`` plugin has these configuration options:

==================== ================================================================================================= ========================================
Parameter            Description                                                                                       Default value
==================== ================================================================================================= ========================================
``sourceDirectory``  The directory containing the documentation source.                                                ``${basedir}/src/site/sphinx``
``outputDirectory``  The directory where the generated output will be placed.                                          ``${project.reporting.outputDirectory}``
``fork``             Whether to run Sphinx in a forked JVM instance.                                                   ``false``
``jvm``              The JVM binary to use. If not set, then will use the one used to run the plugin.
``argLine``          Additional arguments for the forked JVM instance (such as memory options).
``forkTimeoutSec``   How long the plugin should wait for the plugin. 0 means wait forever.                             ``0``
``outputName``       The base name used to create the report's output file(s).                                         ``index``
``name``             The name of the report.                                                                           ``Documentation via sphinx``
``description``      The description of the report.                                                                    ``Documentation via sphinx``
``builder``          The builder to use. See the `Sphinx commandline documentation`_ for a list of possible builders.  ``html``
``verbose``          Whether Sphinx should generate verbose output.                                                    ``true``
``warningsAsErrors`` Whether warnings should be treated as errors.                                                     ``false``
``force``            Whether Sphinx should generate output for all files instead of only the changed ones.             ``false``
``tags``             Additional tags to pass to Sphinx. See the `Sphinx tag documentation`_ for more information.
==================== ================================================================================================= ========================================

A note on memory usage
======================

Sphinx is run via `Jython`_ which will generate lots of small classes for various Python constructs. This means that
the plugin will use a fair amount of memory, especially PermGen space (a moderate plugin run will likely use about 80mb
of PermGen space). Therefore we suggest to either run maven with at least 256mb of heap and 128mb of PermGen space, e.g.

    MAVEN_OPTS="-Xmx256m -XX:MaxPermSize=128m" mvn site

or use the fork parameter of the plugin, e.g.:

.. parsed-literal::

    <plugin>
      <groupId>io.airlift.maven.plugins</groupId>
      <artifactId>sphinx-maven-plugin</artifactId>
      <version>\ |version|\ </version>
      <configuration>
        <fork>true</fork>
        <argLine>-Xmx256m -XX:MaxPermSize=128m</argLine>
      </configuration>
    </plugin>
