Hurricane Labs Content+
===================================

**Hurricane Labs Content+** is a Splunk app that allows for any Splunk Enterprise
search head to subscribe to Hurricane Labs' content repository. This repository
contains Enterprise Security correlation searches, tuning macros, and dashboards that
we have built and continue to build for ourselves and our customers. 

Content is organized into "packages" which each have their own data prerequisites. This
allows the app to only deploy content that is relevant to your particular organization
(see :doc:`compatibility` for more info on this).

Each API key has a monthly "quota" that determines how many packages can be installed
in a given month. See :doc:`quotas` for more info.

Check out the :doc:`usage` section for further information, including
how to :ref:`installation` the project.



Contents
--------

.. toctree::

   usage
   compatibility
   quotas
