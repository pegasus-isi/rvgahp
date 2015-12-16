Reverse GAHP
============

This is an implementation of the remote_gahp for Condor that doesn't require
ssh connections to the remote resource. Instead, it uses connection brokering
to establish bi-directional communication between the GridManager and GAHP
processes running on the remote resource.

Configuration
-------------

On your submit host:

1. In your condor_config set:

    ```
    REMOTE_GAHP = /path/to/rvgahp_proxy
    ```

On the remote resource:

1. Create $HOME/.condor/log
1. Create a $HOME/.condor/condor_config.ft-gahp
1. In condor_config.ft-gahp set:

    ```
    BOSCO_SANDBOX_DIR = $ENV(HOME)/.condor
    LOG = $ENV(HOME)/.condor/log
    FT_GAHP_LOG = $(LOG)/FTGahpLog
    LIBEXEC = /usr/libexec/condor
    ```

1. start the rvgahp_ce process.

Example Job
-----------
```
universe = grid

grid_resource = batch pbs juve@hpc-pegasus.usc.edu
+remote_cerequirements = EXTRA_ARGUMENTS=="-N testjob -l walltime=00:01:00 -l nodes=1:ppn=1"


executable = /bin/date
transfer_executable = False

output = test_$(cluster).$(process).out
error = test_$(cluster).$(process).err
log = test_$(cluster).$(process).log

should_transfer_files = YES
when_to_transfer_output = ON_EXIT
copy_to_spool = false
notification = NEVER

queue 1
```
