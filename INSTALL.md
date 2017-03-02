## Installation

**IMPORTANT: Installation process will differ depending on your SLURM version. This configuration have been tested with SLURM 16.0.9**

Before build the SLURM from sources, make sure that you have resolved all needed dependencies. 
For configuration that is showed below is required these packages (tested on Ubuntu):

	munge curl gcc make bzip2 supervisor python python-dev \
    libmunge-dev libmunge2 lua5.3 lua5.3-dev libopenmpi-dev openmpi-bin \
    gfortran vim python-mpi4py python-numpy python-psutil sudo psmisc \
    software-properties-common python-software-properties iputils-ping \
    openssh-server openssh-client \
    automake autoconf unzip \ 
    libgtk2.0-dev libglib2.0-dev \
    libhdf5-dev \
	libcurl4-openssl-dev
	
After that you should build this plugin. For this `cd' to the directory containing the SLURM package's source code and type:

    $ curl -fsL https://github.com/GRomR1/influxdb-slurm-monitoring/archive/master.zip -o influxdb-slurm-monitoring.zip && \
		unzip -qq influxdb-slurm-monitoring.zip && rm -rf influxdb-slurm-monitoring.zip && \
		sed -i '/src\/plugins\/acct_gather_profile\/none\/Makefile/a\\t\t src\/plugins\/acct_gather_profile\/influxdb\/Makefile' ./configure.ac && \
		cp -r influxdb-slurm-monitoring-master/src/plugins/acct_gather_profile/influxdb/ src/plugins/acct_gather_profile/ && \
		cp -rf influxdb-slurm-monitoring-master/src/plugins/acct_gather_profile/Makefile.am src/plugins/acct_gather_profile/ && \
		rm -rf influxdb-slurm-monitoring-master && \
		./autogen.sh && \
		./configure --with-libcurl && \
		make && make install
    
## Configuration

For any SLURM version the plugin can be enabled and configured through slurm.conf (for example, for task monitoring each 10s):

    AcctGatherProfileType=acct_gather_profile/influxdb
	JobAcctGatherFrequency=task=10
	JobAcctGatherType=jobacct_gather/linux
    
and acct_gather.conf:

	ProfileInfluxDBHost=http://YOUR_INFLUXDB_SERVER:8086
	ProfileInfluxDBDatabase=INFLUXDB_DATABASE_NAME
	ProfileInfluxDBDefault=task
	
If you want get detail information about these options, follow these links: [Profiling Using HDF5 User Guide](https://slurm.schedmd.com/hdf5_profile_user_guide.html),
 [about slurm.conf](https://slurm.schedmd.com/slurm.conf.html).

Make sure that the InfluxDB server is reachable from the SLURM controller host.

All gathered value will be stored in the **default** retention policy.
