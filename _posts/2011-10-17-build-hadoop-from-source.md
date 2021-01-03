---
id: 775
title: Build Hadoop from Source
date: 2011-10-17T23:46:42+00:00
author: tshanky
guid: http://shanky.org/?p=775
permalink: /2011/10/17/build-hadoop-from-source/
dsq_thread_id:
  - 4654810545
categories:
  - 'BigData, NoSQL &amp; Cloud'
  - Ubuntu
---
> _Instructions in this write-up were tested and run successfully on Ubuntu 10.04, 10.10, 11.04, and 11.10. The instructions should run, with minor modifications, on most flavors and variants of Linux and Unix. For example, replacing apt-get with yum should get it working on Fedora and CentOS._ 

If you are starting out with <a title="Apache Hadoop" href="http://hadoop.apache.org/" target="_blank">Hadoop</a>, one of the best ways to get it working on your box is to build it from source. Using stable binary distributions is an option, but a rather risky one. You are likely to not stop at <a title="Hadoop Common" href="http://hadoop.apache.org/common/" target="_blank">Hadoop common</a> but go on to setting up <a title="Apache Pig" href="http://pig.apache.org/" target="_blank">Pig</a> and <a title="Apache Hive" href="http://hive.apache.org/" target="_blank">Hive</a> for analyzing data and may also give <a title="Apache HBase" href="http://hbase.apache.org/" target="_blank">HBase</a> a try. The Hadoop suite of tools suffer from a huge version mismatch and version confusion problem. So much so that many start out with <a title="Cloudera's Distribution (CDH)" href="http://www.cloudera.com/hadoop/" target="_blank">Cloudera&#8217;s distribution, also know as CDH,</a> simply because it solves this version confusion disorder.

Michael Noll&#8217;s well written blog post titled: <a title="Building an Hadoop 0.20.x version for HBase 0.90.2" href="http://www.michael-noll.com/blog/2011/04/14/building-an-hadoop-0-20-x-version-for-hbase-0-90-2/" target="_blank">Building an Hadoop 0.20.x version for HBase 0.90.2</a>, serves as a great starting point for building the Hadoop stack from source. I would recommend you read it and follow along the steps stated in that article to build and install Hadoop common. Early on in the article you are told about a critical problem that HBase faces when run on top of a stable release version of Hadoop. HBase may loose data unless it is running on top an HDFS with durable sync. This important feature is only available in the branch-0.20-append of the Hadoop source and not in any of the release versions.

Assuming you have successfully, followed along Michael&#8217;s guidelines, you should have the hadoop jars built and available in a folder named &#8216;build&#8217; within the folder that contains the Hadoop source. At this stage, its advisable to configure Hadoop and take a test drive.

**Configure Hadoop: Pseudo-distributed mode**

Running Hadoop in pseudo-distributed mode provides a little taste of a cluster install using a single node. The Hadoop infrastructure includes a few daemon processes, namely

  1. HDFS namenode, secondary namenode, and datanode(s)
  2. MapReduce jobtracker and tasktracker(s)

When run on a single node, you can choose to run all these daemon processes within a single Java process (also known as standalone mode) or can run each daemon in a separate Java process (pseudo-distributed mode).

If you go with the pseudo-distributed setup, you will need to provide some minimal custom configuration to your Hadoop install. In Hadoop, the general philosophy is to bundle a default configuration with the source and allow for overriding it using a separate configuration file. For example, hdfs-default.xml, which you can find in the &#8216;src/hdfs&#8217; folder of your Hadoop root folder, contains the default configuration for HDFS properties. The file hdfs-default.xml gets bundled within a compiled and packaged Hadoop jar file and Hadoop uses the configuration specified in this file for setting up HDFS properties. If you need to override any of the HDFS properties that uses the default configuration from hdfs-default.xml, then you need to re-specify the configuration for that property in a file named hdfs-site.xml. This custom configuration definition file, hdfs-site.xml, resides in the &#8216;conf&#8217; folder within the Hadoop root folder. Custom configuration in core-site.xml, hdfs-site.xml, and mapred-site.xml corresponds to default configuration in core-default.xml, hdfs-default.xml, and mapred-default.xml, respectively.

Contents of conf/core-site.xml after custom configuration:

<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0"?&gt;
&lt;?xml-stylesheet type="text/xsl" href="configuration.xsl"?&gt;

&lt;!-- Put site-specific property overrides in this file. --&gt;

&lt;configuration&gt;
  &lt;property&gt;
    &lt;name&gt;fs.default.name&lt;/name&gt;
    &lt;value&gt;hdfs://localhost:9000&lt;/value&gt;
  &lt;/property&gt;
&lt;/configuration&gt;
</pre>

The specified configuration makes the HDFS namenode daemon accessible on port 9000 on localhost.

Contents of conf/hdfs-site.xml after custom configuration:

<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0"?&gt;
&lt;?xml-stylesheet type="text/xsl" href="configuration.xsl"?&gt;

&lt;!-- Put site-specific property overrides in this file. --&gt;

&lt;configuration&gt;
  &lt;property&gt;
    &lt;name&gt;dfs.replication&lt;/name&gt;
    &lt;value&gt;1&lt;/value&gt;
  &lt;/property&gt;
  &lt;property&gt;
    &lt;name&gt;dfs.name.dir&lt;/name&gt;
    &lt;value&gt;path/to/data/dfs/name&lt;/value&gt;
    &lt;description&gt;Determines where on the local filesystem the DFS name node
      should store the name table(fsimage).  If this is a comma-delimited list
      of directories then the name table is replicated in all of the
      directories, for redundancy. &lt;/description&gt;
  &lt;/property&gt;
&lt;/configuration&gt;</pre>

The override specifies a replication factor of 1. On a single node, you can&#8217;t have a failover, can over? The custom configuration also sets the namenode directory to a path on your file system. The default is a folder within your /tmp folder, which gets purged on a restart.

Contents of conf/mapred-site.xml after custom configuration:

<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0"?&gt;
&lt;?xml-stylesheet type="text/xsl" href="configuration.xsl"?&gt;

&lt;!-- Put site-specific property overrides in this file. --&gt;

&lt;configuration&gt;
  &lt;property&gt;
    &lt;name&gt;mapred.job.tracker&lt;/name&gt;
    &lt;value&gt;localhost:9001&lt;/value&gt;
  &lt;/property&gt;
&lt;/configuration&gt;</pre>

After this configuration the MapReduce jobtracker is accessible on port 9001 on localhost.

Finally set JAVA_HOME in conf/hadoop-env.sh. On my Ubuntu 11.10, I have it set as follows:

<pre class="brush: bash; title: ; notranslate" title="">export JAVA_HOME=/usr/lib/jvm/java-6-openjdk</pre>

> If you don&#8217;t have passphraseless ssh setup on your machine then you may need to execute the following commands:
> 
> <pre class="brush: bash; title: ; notranslate" title="">ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub &gt;&gt; ~/.ssh/authorized_keys</pre>
> 
> This creates a DSA encrypted key named id\_dsa and adds it to the set of authorized\_keys for the SSH server running on your localhost. If you aren&#8217;t sure if passphraseless ssh access is setup or not then simply run &#8216;ssh localhost&#8217; on a terminal. If you are prompted for a password then you need to complete the steps to setup passphraseless ssh. 

Now start up the Hadoop daemons using:

<pre class="brush: bash; title: ; notranslate" title="">bin/start-all.sh</pre>

Run this command from the root of your Hadoop folder.

As an additional step set HADOOP\_HOME environment variable to point to the root of your Hadoop folder. HADOOP\_HOME is used by other pieces of software, like HBase, Pig, and Hive, that are built on top of Hadoop.

**Running a Simple Example**
  
If you ran the tests after the Hadoop common install and they passed, then you should be ready to use Hadoop. However, for completeness, I would suggest running a simple Hadoop example while the daemons are up and waiting. Run the simple example illustrated in the official document, available online at <a href="http://hadoop.apache.org/common/docs/r0.20.203.0/single_node_setup.html#PseudoDistributed" title="Hadoop Pseudo-Distribution" target="_blank">http://hadoop.apache.org/common/docs/r0.20.203.0/single_node_setup.html#PseudoDistributed</a>. The example is available at the end of the sub-section on Pseudo-Distributed Operation. 

**Build HBase from Source
  
** 
  
Once Hadoop is up and running, you are ready to build and run HBase. Start by getting the HBase source as follows:

<pre class="brush: bash; title: ; notranslate" title="">git clone https://github.com/apache/hbase.git</pre>

I clone it from the Apache HBase mirror on Github. Alternatively, you can get the source from the HBase svn repository, which is where the official commits are checked-in.

HBase can make use of Snappy compression. Snappy is a fast compression/decompression library, which was built by Google and is available as an open source software library under the &#8216;New BSD License&#8217;. The official site defines snappy as follows:

> Snappy is a compression/decompression library. It does not aim for maximum compression, or compatibility with any other compression library; instead, it aims for very high speeds and reasonable compression. For instance, compared to the fastest mode of zlib, Snappy is an order of magnitude faster for most inputs, but the resulting compressed files are anywhere from 20% to 100% bigger. On a single core of a Core i7 processor in 64-bit mode, Snappy compresses at about 250 MB/sec or more and decompresses at about 500 MB/sec or more.
> 
> Snappy is widely used inside Google, in everything from BigTable and MapReduce to our internal RPC systems. (Snappy has previously been referred to as “Zippy” in some presentations and the likes.) 

You can learn more about Snappy at <a href="http://code.google.com/p/snappy/" title="snappy -- a fast compressor/decompressor" target="_blank">http://code.google.com/p/snappy/</a>.

To build HBase with snappy support we need to do the following:

  1. Build and install the snappy library 
  2. Build and install hadoop-snappy, the library that bridges snappy and Hadoop 
  3. Compile HBase with snappy

**Build and Install snappy**
  
Building and installing snappy is easy and quick. Get the latest stable snappy release as follows:

<pre class="brush: bash; title: ; notranslate" title="">wget http://snappy.googlecode.com/files/snappy-1.0.4.tar.gz</pre>

The current latest release version is 1.0.4. This version number could vary as newer versions supersede this version.
  
Once you download the snappy zipped tarball, extract it:

<pre class="brush: bash; title: ; notranslate" title="">tar zxvf snappy-1.0.4.tar.gz</pre>

Next, change into the snappy extracted folder and use the common configure, make, make install trio to complete the build and install process.

<pre class="brush: bash; title: ; notranslate" title="">cd snappy-1.0.4
./configure && make && sudo make install</pre>

You may need to run &#8216;make install&#8217; using the privileges of a superuser, i.e. &#8216;sudo make install&#8217;.

**Build and Install hadoop-snappy**
  
Hadoop-snappy is a project for Hadoop that provides access to the snappy compression/decompression library. You can learn details about hadoop-snappy at <a href="http://code.google.com/p/hadoop-snappy/" title="Hadoop-snappy" target="_blank">http://code.google.com/p/hadoop-snappy/</a>. Building and installing haddop-snappy requires Maven. To get started checkout the hadoop-snappy code from its subversion repository like so:

<pre class="brush: bash; title: ; notranslate" title="">svn checkout http://hadoop-snappy.googlecode.com/svn/trunk/ hadoop-snappy-read-only</pre>

Then, change to the &#8216;hadoop-snappy-read-only&#8217; folder and make a small modification to maven/build-compilenative.xml :

<pre class="brush: xml; title: ; notranslate" title=""># add JAVA_HOME as an env var
&lt;exec dir="${native.staging.dir}" executable="sh" failonerror="true"&gt;
    &lt;env key="OS_NAME" value="${os.name}"/&gt;
    &lt;env key="OS_ARCH" value="${os.arch}"/&gt;
    &lt;env key="JVM_DATA_MODEL" value="${sun.arch.data.model}"/&gt;
    &lt;env key="JAVA_HOME" value="/usr/lib/jvm/java-6-openjdk"/&gt;
    &lt;arg line="configure ${native.configure.options}"/&gt;
&lt;/exec&gt; 
</pre>

Also, install a few required zlibc related libraries:

<pre class="brush: bash; title: ; notranslate" title="">sudo apt-get install zlibc zlib1g zlib1g-dev[/pre]
Next, build Hadoop-snappy using maven like so:
1sudo mvn package</pre>

Once hadoop-snappy is built, install the jar and tar distributions of hadoop-snappy to your local repository:

<pre class="brush: bash; title: ; notranslate" title="">mvn install:install-file -DgroupId=org.apache.hadoop -DartifactId=hadoop-snappy -Dversion=0.0.1-SNAPSHOT -Dpackaging=jar -Dfile=./target/hadoop-snappy-0.0.1-SNAPSHOT.jar
mvn install:install-file -DgroupId=org.apache.hadoop -DartifactId=hadoop-snappy -Dversion=0.0.1-SNAPSHOT -Dclassifier=Linux-amd64-64 -Dpackaging=tar -Dfile=./target/hadoop-snappy-0.0.1-SNAPSHOT-Linux-amd64-64.tar 
</pre>

**Compile, Configure & Run HBase**
  
Once snappy and hadoop-snappy are compiled and installed, you are ready to compile HBase with snappy support. Change to the folder that contains the HBase repository clone and run the &#8216;maven compile&#8217; command to build HBase from source.

<pre class="brush: bash; title: ; notranslate" title="">cd hbase
mvn compile -Dsnappy</pre>

The -Dsnappy option tells maven to compile HBase with snappy support.

Earlier, I setup Hadoop to run in pseudo-distributed mode. Lets configure HBase to also run in pseudo-distributed mode. Alike Hadoop, the default configuration for HBase is available in hbase-default.xml and custom configuration can be specified to override the default configuration. Custom configuration resides in conf/hbase-site.xml. To setup, HBase in pseudo-distributed make sure the contents of conf/hbase-site.xml are as follows:

<pre class="brush: xml; title: ; notranslate" title="">&lt;configuration&gt;
    &lt;property&gt;
        &lt;name&gt;hbase.rootdir&lt;/name&gt;
        &lt;value&gt;hdfs://localhost:9000/hbase&lt;/value&gt;
        &lt;description&gt;The directory shared by RegionServers.
        &lt;/description&gt;
    &lt;/property&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.replication&lt;/name&gt;
        &lt;value&gt;1&lt;/value&gt;
        &lt;description&gt;The replication count for HLog and HFile storage.
        Should not be greater than HDFS datanode count.
        &lt;/description&gt;
    &lt;/property&gt;
&lt;/configuration&gt;
</pre>

You may recall we configured the HDFS namenode to be accessible on port 9000 on localhost. Therefore, the &#8216;hbase.rootdir&#8217; needs to be specified with respect to the HDFS url. If you configure to run HDFS daemons on a different port, then please adjust the configuration for &#8216;hbase.rootdir&#8217; in line with that. The second custom property definition sets replication factor value to 1. On a single node thats the best and only option you have!

Now you can start-up Hbase using:

<pre class="brush: bash; title: ; notranslate" title="">bin/start-hbase.sh
</pre>

**Build Pig**
  
Building and Installing Pig from source is a simple 3 command operation like so:

<pre class="brush: bash; title: ; notranslate" title="">svn checkout http://svn.apache.org/repos/asf/pig/trunk/ pig
cd pig
ant
</pre>

The first command checks out source from the Pig svn repository. It grabs the source from the &#8216;trunk&#8217;, which is referred to as &#8216;master&#8217; in git jargon. The second command changes to the folder that contains the pig source. The third command compiles the pig source using Apache Ant. Invoking the default target, i.e. simply calling &#8216;ant&#8217; without any argument, compiles Pig and packages it as a jar for distribution and consumption. Pig jar file can be found at the root of the Pig folder. Pig usually generates two jar files:

  1. pig.jar &#8212; to be run with Hadoop.
  2. pigwithouthadoop.jar &#8212; to be run locally. Pig does not need to always use Hadoop.

**Build Hive**
  
Building and Installing Hive is almost as easy as building and installing Pig. The following set of commands gets the job done:

<pre class="brush: bash; title: ; notranslate" title="">svn co http://svn.apache.org/repos/asf/hive/trunk hive
cd hive
ant package
</pre>

You should be able to understand the commands if you have come so far in this article. 

There is one little catch in this Hive instruction set though. As you run the &#8216;ant package&#8217; task you will see the build fail. With HADOOP_HOME pointing to a hadoop-0.20-append branch build, Hive ShimLoader does not get the Hadoop version correctly. Its the &#8220;-&#8221; in the name that causes the problem! Apply, the simple patch available at <a href="https://issues.apache.org/jira/browse/HIVE-2294" title="[#HIVE-2294]" target="_blank">https://issues.apache.org/jira/browse/HIVE-2294</a> and things should work just fine. Apply the patch as follows:

<pre class="brush: bash; title: ; notranslate" title="">patch -p0 -i HIVE-2294.3.patch</pre>

To start using Hive, you will also need to minimally carry out these additional tasks:

  * Set HIVE_HOME environment variable to point to the root of the HIVE directory. 
  * Add $HIVE_HOME/bin to $PATH 
  * Create /tmp in HDFS and set appropriate permissions 

<pre class="brush: bash; title: ; notranslate" title="">bin/hadoop fs -mkdir /tmp 
bin/hadoop fs -chmod g+w   /tmp
</pre>

  * Create /user/hive/warehouse and set appropriate permissions 

<pre class="brush: bash; title: ; notranslate" title="">bin/hadoop fs -mkdir /user/hive/warehouse 
bin/hadoop fs -chmod g+w /user/hive/warehouse
</pre>

Now you are ready with pseduo-distributed Hadoop, pseudo-distributed HBase, Pig, and Hive running on your box. This is of course just the beginning. You need to learn to leverage these tools to analyze data, but that not covered in this write-up. A following post will possibly address the topic of analyzing data using MapReduce and its abstractions.