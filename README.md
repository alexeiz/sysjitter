**Note**: this is a fork of https://github.com/Xilinx-CNS/cns-sysjitter.  I made it for two reasons:

* the original code wasn't available on github at the time (it was disbributed as a tarball),
* I made some changes to make it work with `taskset` (which unintentionally broke the `--cores` option).

The original readme follows:

---

# sysjitter v1.4

 Copyright 2010-2017 David Riddoch <david@riddoch.org.uk>

 sysjitter measures the extent to which the system impacts on user-level
 code by causing jitter.  It runs a thread on each processor core, and when
 the thread is "knocked off" the core it measures how long for.  At the end
 of the run it outputs some summary statistics for each core, and
 optionally the full raw data.

Prerequisites
-------------

- Supports x86, x86_64 and Power processors.

- Requires a version of gcc with support for built-in atomics.

Building
--------

    make

Running
-------

  It is important to run sysjitter on cores that are idle.  If running on a
  system with cpusets enabled, then run sysjitter as root.

    sysjitter <threshold-nsec>

  The argument <threshold-nsec> tells sysjitter to ignore any
  "interruptions" shorter than the value given.  Something upwards of a few
  hundred nanoseconds is a reasonable choice here.

  The default output gives summary statistics for each CPU core.  Pipe the
  output through "column -t" to make it a little easier on the eye.

  You can get the raw data out with the --raw option.  This includes the
  timestamp and duration of each interruption on each CPU core.  The data
  is placed in a separate file for each CPU core.

  By default it will run for 70 seconds.  Override with the --runtime
  option.  e.g. For a 10 second run:

    sysjitter --runtime 10 300

  You can run on a specified subset of cores, given as a comma separated
  list of cores and/or ranges.  This example runs on cores 2, 4, 5 and 6
  and reports interruptions of at least 20us:

    sysjitter --cores 2,4-6 20000

  Note that the cpu_mhz row is misnamed; this is actually the measured tick
  rate of the CPU timestamp counter (in MHz), which may or may not be the
  same as the clock frequency.
