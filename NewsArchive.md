### March 29th, 2010 ###
HBase-Writer 0.9-SNAPSHOT has now been released.  This version is compatible with both Heritrix 2.X and Heritrix 3.X.  Much thanks to [Greg Lu](http://www.greglu.com/) for spearheading this effort and sending in the initial patch.  Once Heritrix has an official 3.0.0-RELEASE, then HBase-writer will release version 0.9-RELEASE.  Thanks again Greg!

### October 18th, 2009 ###
HBase-Writer 0.20.3 has now been released.  Thanks to the patch sent in by Joost Ouwerkerk, a major bug was discovered and fixed; the issue was that replayInputStream was not being closed properly after hbase writer wrote the raw content to the hbase table.  As a result, there was a steady growing number of open stream objects in memory as the crawler ran.  This has been fixed by manually forcing a close of the stream object.  Thanks again Joost!

### October 14th 2009 ###
HBase-Writer 0.20.2 has now been released.  A new runtime jar dependency has been added: zookeeper.jar.  This is because HBase 0.20.X now depends on the Zookeeper service layer to manage the connections to the HBase master.  This change is significant for the client side because you no longer require the HBase master address to access your HBase tables, but instead you need the list of hosts serving in the zookeeper quorum.  The new input is now a comma-separated list of these zk quorum hosts in the global sheet for the HBaseWriter Processor configuration.  These zk hosts are analogous to the values set for the  "hbase.zookeeper.quorum" property in hbase-site.xml.  I have also added support for the hbase property: "hbase.zookeeper.property.clientPort" if you happen to run your zk quorum on a non-standard port.  Enjoy.

### October 13th 2009 ###
Upgraded and now testing the trunk on Hbase 0.20.1, and at the moment I am having ZooKeeper configuration issues.  Once I work these out, I'll be updating the new dependency list which will include the zookeeper jar.  Wish me luck! :)  Feel free to download the hbase-wrtier-0.20.0-SNAPSHOT jar from the archive repository manager.  When it is fully tested and working, I will release 0.20.

### October 8th 2009 ###
HBase 0.20.0 has been released.  I am testing hbase-writer on hbase-0.20.0 cluster today and should have a 0.20.0 release of hbase-writer by the end of the week.

### Feb. 16th 2009 ###
HBase-Writer version 0.19.1 has been released.  This version fixes the new feature added in 0.19.0: "only\_new\_records".  After 0.19.0 was released, it was realized that since content was not being downloaded by Heritrix from the webserver when 'only\_new\_records' was set to "true", then Heritrix couldnt follow any more links on sites that were partially crawled.  So  it was then discovered that Heritrix needs to download the content to pages already crawled to get the new links to crawl (or save the state of the crawl before ending the crawl).  So this problem is probably best handled by Heritrix itself by taking snapshots during the crawl or overriding the extractor classes in Heritrix to save parsed urls.  So hbase-writer 0.19.1 will download all urls found by Heritrix, and only write new records to the Hbase table when 'only\_new\_records' is set to "true".  Default is "false" which is to write all crawled urls as the same row key records with different (new) timestamped cells in the given HBase table on every crawl.  Compiled on 1.6, Enjoy.


### Feb. 11th 2009 ###
HBase-Writer version 0.19.0 has been released.  This version has been tested on a few crawls, large and small and works on hadoop & hbase 0.19.0.  A new feature has been added: only-new-records.   This boolean is set to "false" by default. In default mode, heritrix will crawl all urls regardless of existing rowkeys (urls). By setting this to "true" you ensure that only new urls(rowkeys) are written to the crawl table.  Also please note, this version of hadoop now requires Java 1.6 (http://hadoop.apache.org/core/docs/r0.19.0/releasenotes.html - first item) so hbase-writer-0.19.X now requires Java 1.6 as well.   Enjoy.

### Feb. 2nd 2009 ###
HBase has released 0.19.0 on January 21st 2009.

HBase-Writer 0.19-SNAPSHOT is being tested against hbase-0.19.0 release and hadoop-0.19.0 release.  Once a few runs have been done, I will release hbase-writer 0.19.0.