# Introduction

This role deploys a system to automatically update Debian hosts.  It also
applies package manager configuration.


# Major Version Upgrades

I get this eror when incrementing the release name from `bookworm` to `trixie`,
at the `apt update` step:

    fatal: [synapse]: FAILED! => {"changed": false, "msg": "E:The value 'trixie' is invalid for APT::Default-Release as such a release is not available in the sources"}

Running `apt update` by hand seems to work fine.  Running
`update_debian_cronjob` after that does not give a similar error to above.
There does tend to be a long delay and/or crash as aptitude tries to resolve
dependencies.

It might be wise to provide some human intervention during major version
upgrades.


# BUGS

There is a step to verify that updates run, but note that the step will
succeed, even if updates fail, as long as a log is created.  Monitoring should
notify about any failure, later.  However, for example, if the update process
reports failure because reboot is required, that is still a successful
deployment.  This role could probably check the error code and fail on other
error types, but as of now failure is simply ignored, as long as it is logged.

The role could, arguably, be called "apt" or "apt-client" or something like
that, as any changes to apt configuration end up here, currently.  The role
might also be split, and this role could depend on a separate role that just
configures apt.

