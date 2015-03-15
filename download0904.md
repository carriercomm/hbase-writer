# Hosted by OpenSourceMasters #

[OpenSource Masters.com is hosting the public artifact repository.](http://opensourcemasters.com/)

# Download Link #
  * [Click here to download HBase Writer 0.90.4](http://repo.opensourcemasters.com:8080/nexus/content/repositories/releases/org/archive/hbase-writer/0.90.4/)
  * Release 0.90.4 - 23.1.2012
  * Added support to change max file size.
  * Major bug fix submitted by Greg Lu, all the usages of the connection pooling functionality built into Heritrix were not being used and instead HBase-writer was creating a new HTable objects indirectly for every page being saved.  This has been fixed.  Even worse, the connections weren't being cleaned up properly and would stick around until the process was shut down.  The connections are now being closed.  Thanks again to Greg Lu for the work done to fix this problem.  (Patch submitted by Greg Lu via Ryan Smith)
  * Moved all configuration options to HBaseParameters class and cleaned up naming.	(Patch submitted by Greg Lu via Ryan Smith)
  * HBaseWriterPool has been added back extending WriterPool from Heritrix, No longer using WARCWriterPool.  (Patch submitted by Greg Lu via Ryan Smith)