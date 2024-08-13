Compatibility
=============

The "HLCP Compatibility Check" saved search is how we determine whether a given content
package is or is not compatible. There are multiple different types of compatibility
specifications that a package can make:

- Splunk app dependencies: Apps the must be installed and at a specified version in
  order for a given package to work properly.
- Data dependencies: Sources, sourcetypes and datamodels, that must have data as well
  as arbitrary search strings that must return events in order for a given package to
  work properly.
- Package dependencies: Other packages which need to be installed already for a given
  package to work properly. These are checked for compatibility as well, recursively.

When it's time to check a package for compatibility, checks are run in the below order.
This is intentionally ordered from most efficient to least efficient, because if any
checks fails the compatibility check will finish prematurely.

#. Splunk app compatibilitiy
#. Sourcetypes
#. Sources
#. Datamodels
#. Arbitrary searches
#. Package dependencies (go to step 1)

The above-mentioned "short circuit" logic is one optimization to avoid putting a large
amount of load on Splunk, but this command goes further than that. Additionally, the
results of all the above checks are cached in the KV store so that they don't need to
be checked more than once. At the time of writing the "ttl" of that cache is 7 days.
This is saved between checks as well, so even if 10 packages need to be checked for a
given datamodel, that check will at most be performed once.

The last performance-related decision this command makes is a per-run search limit
(currently set to 6 searches). Because many of our cloud customers have strict SVC
usage limits, it's important that if we have a lot of searches to run (in the case of a
new batch of packages being added to the repo, or a fresh app install) we don't run
them all at once. If hlcpcheck hits this search limit, it will prematurely terminate.
Because checks are cached, the next time it runs the search command won't need to run
the same set of searches. This way over many runs, the command will naturally space out
its backlog of dependency checks.

Package compatibility information is stored in the hlcp_package_compatibility KV store
collection.

