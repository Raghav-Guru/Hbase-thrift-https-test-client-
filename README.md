# Hbase-thrift-https-test-client-
This  class is  derived from HttpDoAsClient class provided in hbase-example jar , modified to support https option :

**Step 1:** Clone the repo :

    #cd /var/tmp
    #git clone https://github.com/Raghav-Guru/Hbase-thrift-https-test-client-.git
    #cd /var/tmp/Hbase-thrift-https-test-client-
    

**Step 2:** Compile using below command : 
    For HDP : 
    #$JAVA_HOME/bin/javac -cp `hadoop classpath`:/usr/hdp/current/hbase-client/lib/* HttpsDoAsClient.java
    
    For CDH :
    #$JAVA_HOME/bin/javac -cp `hadoop classpath`:/opt/cloudera/parcels/CDH/jars/* HttpsDoAsClient.java
 

**Step 3a:** If kerberized cluster, kinit as hbase user :

    #kinit -kt /etc/security/keytabs/hbase.service.keytab hbase/`hostname -f`

**Step 3b:** Execute using below command: 

    #$JAVA_HOME/bin/java -cp .:/usr/hdp/current/hbase-client/lib/*:`hadoop classpath` HttpsDoAsClient <Hostname> <Port> <doAsUser> true https

> (Note: Need to import certificate chain of Hbase thrift server to java
> cacerts if ssl enabled on thrift or alternatively to use different
> truststore, create new truststore with hbase thrift cert chain and
> pass truststore arguments in command line
> -Djavax.net.ssl.trustStore="TrusStorePath" -Djavax.net.ssl.trustStorePassword="password")

Eg: 

    # /usr/jdk64/jdk1.8.0_112/bin/java -cp .:/usr/hdp/current/hbase-client/lib/*:`hadoop classpath` HttpsDoAsClient c416-node3.raghav.com 9090 hbase true https
    Hbase Thrift URL:https://c416-node3.raghav.com:9090
    scanning tables...
      found: ATLAS_ENTITY_AUDIT_EVENTS
      found: SYSTEM.CATALOG
      found: SYSTEM.FUNCTION
      found: SYSTEM.SEQUENCE
      found: SYSTEM.STATS
      found: atlas_titan
      found: atlas_titan_new
      found: demo_table
        disabling table: demo_table
        deleting table: demo_table
    creating table: demo_table
    column families in demo_table:
      column: entry:, maxVer: 10
      column: unused:, maxVer: 3
     

  
  Eg: With trustStore system Options: 
  

       # echo | openssl s_client -connect localhost:9090 2>&1 |  sed --quiet '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > hbase_thrift.crt
       # /usr/jdk64/jdk1.8.0_112/bin/keytool -import -keystore hbase_thrift_trusstore.jks -file  hbase_thrift.crt -alias hbase_thrift -storepass hadoop
       # /usr/jdk64/jdk1.8.0_112/bin/java -Djavax.net.ssl.trustStore=hbase_thrift_trusstore.jks  -Djavax.net.ssl.trustStorePassword=hadoop -cp .:/usr/hdp/current/hbase-client/lib/*:`hadoop classpath` HttpsDoAsClient c416-node3.raghav.com 9090 hbase true https
      
