Welcome to HBase-Writer README for Heritrix 2.  

This document can also be found online here:
http://code.google.com/p/hbase-writer/wiki/READMEHeritrix2

Specific versions of HBase-Writer now support different
version combinations of Heritrix and HBase. Please refer to
http://code.google.com/p/hbase-writer/wiki/VERSIONS
for a more detailed list.

Before reading this document, please make sure the
HBase-Writer version you downloaded is meant to work with
Heritrix 2.


= TABLE OF CONTENTS =
 
  * SETUP
  * CONFIGURING HERITRIX
  * FILE FORMAT
  * COMPILING THE SOURCE
  * BUILDING THE JAR
  * BUILDING THE SITE-REPORT

The hbase-writer is an extension to the Heritrix open
source crawler written by the Internet Archive (http://crawler.archive.org/)
that enables it to store crawled content directly into HBase, the Hadoop
Distributed Database (http://hbase.org/  http://hadoop.apache.org/).  Hadoop implements
the Map/Reduce distributed computation framework on top of HDFS.  
hbase-writer-processor writes crawled content into hbase table records with rowkeys.
Having the crawled data in hbase tables is directly supported by the Map/Reduce framework.  
This facilitates running high-speed, distributed computations over content crawled with Heritrix.


= QUICK SETUP = 

1. Start an instance of hbase.

2. Install heritrix-2.x.x

3. Copy the following jar files into the ${HERITRIX_HOME}/lib directory from ${HBASE_HOME}/lib:

  hbase-writer-x.x.x.jar
  
  hbase-x.x.x.jar
  
  zookeeper-x.x.x.jar
  
  hadoop-x.x.x-core.jar
  
  log4j-x.x.x.jar
  
  protobuf-java-x.x.x.jar 
  
  commons-configuration-x.x.x.jar 

  hbase-protocol-x.x.x-hadoopX.jar 
  
  slf4j-api-x.x.x.jar
  
  htrace-core-x.x.x.jar

  jackson-core-asl-x.x.x.jar 

4. Start Heritrix


= CONFIGURING HERITRIX = 

On the "Settings for sheet 'global'":

- Click on a 'details' link and add new processor com.powerset.heritrix.writer.HBaseWriterProcessor.
- Return to the global sheet and set this as your writer (the page will not draw completely if
did not type in the name of the processor properly -- see heritrix_out.log for errors).
- Set at least the zkquorum and table configuration for HBaseWriterProcessor.
	hbaseTableName (required)
	  Which table in HBase to write the crawl to.  This table will be created automatically if it doesnt exist.
	  e.g. crawl

	zkQuorum (required)
	  The zookeeper quroum that serves the hbase master address.  Since hbase-0.20.0, the master server's address is returned by the zookeeper quorum.
	  So this value is a comma seperated list of the zk quorum.
	  e.g. zkHost1,zkHost2,zkHost3

	zkPort (optional, defaults to 2181, which is the zookeeper default)
	  The zookeeper quroum client port that clients should connect to to get HBase information.
	  e.g. 2181

	onlyWriteNewRecords (optional, defaults to 'false')
	  Set to "false" by default.  In default mode, heritrix will write all urls to hbase regardless of existing table rowkeys (urls).
	  By setting this to "true" you ensure that only new urls(rowkeys) are written to the crawl table.

	onlyProcessNewRecords (optional, defaults to 'false')
	  Set to "false" by default.  In default mode, heritrix will process (fetch from server and read) all urls regardless of existing rowkeys (urls).
	  By setting this to "true" you ensure that only new urls(rowkeys) are processed by heritrix.  Also, if set to "true",
	  heritrix doesnt download any content that is already existing as a record in the hbase table.
	  
	defaultMaxFileSizeInBytes (optional, defaults to 20*1024*1024)
	  Set to 20MB (20*1024*1024 bytes) by default.  If data item is fetched and it exceeds this amount, the content will not be written to hbase.  
	  However, an annotation cell will be written to log that this was not written due to max size limit configuration.

	contentColumnFamily (optional)
	  The column family name for where you want to save the content to. Defaults to "n".

	contentColumnName (optional)
	  The column qualifier name for where you want to save the content to. Defaults to "raw" , full column name i.e. "n:raw"

	curiColumnFamily (optional)
	  The column family name for storing the Crawl URI related information. Defaults to "c".

	ipColumnName (optional)
	  The column qualifier name for storing the IP address. Defaults to "ip" which becomes "c:ip".

	pathFromSeedColumnName (optional)
	  The column qualifier name for storing the path from seed. Defaults to "pfs" which becomes "c:pfs".

	isSeedColumnName (optional)
	  The column qualifier name for storing whether the crawl is a seed. Defaults to "is" which becomes "c:is".

	viaColumnName (optional)
	  The column qualifier name for storing where it came from. Defaults to "via" which becomes "c:via".

	urlColumnName (optional)
	  The column qualifier name for storing the URL. Defaults to "url" which becomes "c:url".

	requestColumnName (optional)
	  The column qualifier name for storing the request. Defaults to "req" which becomes "c:req".
	  
	contentTypeColumnName (optional)
		The column qualifier name for storing the content type. Defaults to "ct" which becomes "c:ct".

	contentSizeColumnName (optional)
		The column qualifier name for storing the content size. Defaults to "cz" which becomes "c:cz".

	contentLengthColumnName (optional)
		The column qualifier name for storing the content length. Defaults to "cl" which becomes "c:cl".

	fetchAttmptsColumnName (optional)
		The column qualifier name for storing the fetch attempts. Defaults to "fa" which becomes "c:fa".

	fetchDurationColumnName (optional)
		The column qualifier name for storing the fetch duration. Defaults to "fd" which becomes "c:fd".

	fetchAnnotationsColumnName (optional)
		The column qualifier name for storing the annotations that occurred during the fetch. Defaults to "an" which becomes "c:an".

	fetchAnnotationsValueDelimiter (optional)
		The column qualifier name for storing the request. Defaults to ", "
