Quotas
=======

Each API key has a monthly "quota" that determines how many packages can be installed.
If you attempt to deploy more than this number in a one-month time frame, the
``hlcpinstall`` command will return an error. Your monthly usage is reset on the first
of each month at 00:00 UTC.
To view your quota as well as your current monthly usage, you can either look at the
top-right corner of the "Content Overview" dashboard, or run the ``hlcpquota``
command.