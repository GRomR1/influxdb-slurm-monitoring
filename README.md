## Introduction

The **acct_gather_profile/influxdb** plugin uses the same base as the [HDF5 profiling plugin](https://slurm.schedmd.com/hdf5_profile_user_guide.html). It allows Slurm to coordinate collecting data on jobs it runs on a cluster that is more detailed than is practical to include in its database. The data comes from periodically sampling various performance data either collected by Slurm, the operating system, or component software. The plugin will record the data from each source as a Time Series into a custom [InfluxDB](https://docs.influxdata.com/influxdb) server.

Collects exactly the same information as the HDF5 plugin:

Measurement | Description
------------ | -------------
CPUFrequency	|   CPU Frequency at time of sample
CPUTime	|   Seconds of CPU time used during the sample
CPUUtilization	|   CPU Utilization during the interval
Pages	|   Pages used in sample
ReadMB	|   Number of megabytes read from local disk
RSS	|   Value of RSS at time of sample
VMSize	|   Value of VM Size at time of sample
WriteMB	|   Number of megabytes written to local disk

A small buffer (16KB) is used to avoid sending data for every sample collected. After task ended, plugin will send buffered data.

Information is sent to the central server using **libcurl-devel** library, so you should use this **configure** option:

    --with-libcurl

It is a good idea to have a web layer over your InfluxDB server, such as [Grafana](http://grafana.org/), in order to visualize the data.

Here you can find some [Screenshots](https://github.com/GRomR1/influxdb-slurm-monitoring/screenshots).

Please, refer to  [INSTALL.md](https://github.com/asanchez1987/jobcomp-elasticsearch/blob/master/INSTALL.md) for installation instructions.