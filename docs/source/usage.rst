Usage
=====

.. _installation:

Requirements
------------
HurricaneLabsContentPlus is compatible with all Splunk Enterprise versions newer than
9.0.

This app is intended for use in conjunction with Splunk Enterprise Security and should
only be installed on a search head with Enterprise Security already installed and setup.

Installation
------------

To use this app, you will need an API key. If you don't have an API key, please reach
out to splunk-app@hurricanelabs.com. 

First, install the Splunk app on your Enterprise Security search head. You can install
it directly from Splunkbase by browsing to Apps->Manage Apps->Browse More Apps, or you
can download the app tarball from `Splunkbase`_.

Once the app is installed you will be redirected to the app's setup page and asked to
configure it. 

- API key: Enter your API key here. If you don't have one, contact
  splunk-app@hurricanelabs.com.
- HL alerting type: If you are not an HL managed SOC customer, set this to "None". 
  If you are configuring this app yourself, chances are you should set this to "None".
- Automatically Installed Content Filter: This is an advanced feature that you probably
  don't need to touch. If you have the "HLCP Monthly Content Feed" search enabled to
  automatically deploy content on the first of each month, you can use this option to
  filter it. There are a couple of presets as well as an option to modify the macros
  directly.

Once you click "submit", the setup page will verify your API key and if that succeeds,
will grab the latest package catalog and redirect you to the content overview page. 
At this point, you will be able to see packages available in our catalog, however
compatibility checking is still in progress. The initial compatibility checks this app
does can be search-intensive, numbering in the dozens of searches. Because many of our
customers are on Splunk Cloud and pay for workload-based licenses, we intentionally
limit how many searches are run at a time. As a result of this, the first set of
compatibility checks can take 1-2 hours. Refresh the Content Overview dashboard to see
compatibility checking status for all packages. 

Content Overview
----------------

This dashboard allows you to browse the Content+ content catalog, view what content
packages are currently deployed, and view what packages are compatible with your
environment. If a content package is not already-deployed and is compatible, there
will be a "Deploy" button that will allow you deploy it. Deploying a package will
increase your quota usage by 1 for the month. You can see your current quota in the
top right corner of the dashboard.

Manual Usage
----------------

The underlying functionality of this app is built using search commands. If you
prefer, you can use them instead of the Content Overview dashboard. Some example
scenarios are listed below.

Install
########

If you want to install a single package manually, run the following. If the package is
not compatible or a duplicate is found, this will fail and let you know. package_ids
can be a comma-delimited list.
``| hlcpinstall package_ids=<the_package_id>``


Check a package's compatibility
###############################
Let's say you just recently onboarded a new sourcetype that was required for a package
and you want to update its compatibility status. The below command will re-run all
compatibility checks for a specific package. The force flag is necessary in order to
invalidate cached results.
``| hlcpcheck package_ids=<the_package_id> force=1``


Grab the latest version of the Content+ catalog
###############################################
This is done on an hourly basis already by the "HLCP Catalog Update" search. However it
can be done ad-hoc with the ``| hlcpfetch`` command. There are no parameters and the
output is stored inside the ``hlcp_catalog`` collection.


Check your API key's monthly usage and number of permitted monthly packages
############################################################################
``| hlcpquota``. No parameters and output is returned directly as a search result.

Fork a package
###############

With the exception of macros, any change to the contents of a package will be
overwritten when Hurricane Labs pushes out a new update. Most of the time, this is a
useful thing as it allows us to improve content transparently and frictionlessly.
However there may be situations where you want to make significant modifications to
a search and prevent the app from updating it. In these situations, you can "fork" the
package using the following macro:

``| `hlcp_fork(package_id)```

Where package_id is the ID of your package (you can pull this from the Content Overview
dashboard). 

If a later date, you decide you'd rather just pull the stock package contents, you can
un-fork the package like so:

``| `hlcp_unfork(package_id)```

.. _Splunkbase: https://splunkbase.splunk.com/app/7258