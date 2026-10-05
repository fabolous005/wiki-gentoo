<!-- source: https://wiki.gentoo.org/wiki/Eclipse/Building_From_Source | group: Gentoo Wiki (Main) | wiki-title: Eclipse/Building From Source -->
---
title: Eclipse/Building From Source
url: https://wiki.gentoo.org/wiki/Eclipse/Building_From_Source
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: "29aae9884dc361fe"
license: CC BY-SA 4.0
---

# Eclipse/Building From Source

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Work in Progress

Participation wanted (and essential)

Ideas behind this table:

- The list of jar files was found using "find -name '\*.class' -o -name '\*.jar' | fgrep -v org.eclipse | sort"
- Indentation is done using " ", one per level, e.g. dependencies of dependencies go two levels deep
- IDs are used to resolve duplication.

If you feel like contributing ebuilds, please

- start with the ones missing altogether
- document the dependencies you find in the table below
- clone [https://github.com/gentoo/eclipse-overlay](https://github.com/gentoo/eclipse-overlay) and send them in as pull requests
- also, be invited to review other pull requests as well

| ID | Filename | Gentoo package | Version needed | Exact version present in overlay | Other versions present in | Comments | 
|---|---|---|---|---|---|---|
| 1 | eclipse-java-mars-R-linux-gtk-x86\_64.tar.gz | dev-util/eclipse-sdk | 4.5 |  | gentoo: 3.5.1 |  | 
| 2 | eclipse/plugins/ch.qos.logback.classic\_1.0.7.v20121108-1250.jar | dev-java/logback | 1.0.7 |  | gentoo: 1.0.13 | [Source .tar.gz](http://logback.qos.ch/dist/logback-1.0.7.tar.gz) | 
| 3 | eclipse/plugins/ch.qos.logback.core\_1.0.7.v20121108-1250.jar | dev-java/logback | 1.0.7 |  | gentoo: 1.0.13 | [Source .tar.gz](http://logback.qos.ch/dist/logback-1.0.7.tar.gz) | 
| 4 | eclipse/plugins/ch.qos.logback.slf4j\_1.0.7.v201505121915.jar | dev-java/logback-slf4j | 1.0.7 |  |  | [Source .jar](http://download.eclipse.org/recommenders/updates/stable/plugins/ch.qos.logback.slf4j.source_1.0.7.v201505121915.jar) | 
| 5 | eclipse/plugins/com.google.gson\_2.2.4.v201311231704.jar | dev-java/gson | 2.2.4 |  | gentoo: 2.3.1 |  | 
| 6 | eclipse/plugins/com.google.guava\_15.0.0.v201403281430.jar | dev-java/guava | 15.0.0 | gentoo |  |  | 
| 7 | eclipse/plugins/com.google.inject\_3.0.0.v201312141243.jar | dev-java/guice | 3.0 | gentoo |  |  | 
| 8 | eclipse/plugins/com.google.inject.multibindings\_3.0.0.v201402270930.jar | dev-java/guice | 3.0 | (gentoo) |  | Ebuild not yet installing related files | 
| 9 | eclipse/plugins/com.ibm.icu\_54.1.1.v201501272100.jar | dev-java/icu4j | 54.1.1 | gentoo |  |  | 
| 10 | eclipse/plugins/com.jcraft.jsch\_0.1.51.v201410302000.jar | dev-java/jsch | 0.1.51 |  | gentoo: 0.1.49, 0.1.52 |  | 
| 11 | eclipse/plugins/com.sun.el\_2.2.0.v201303151357.jar | dev-java/el-ri | 2.2 |  |  | [Sources](http://download.eclipse.org/tools/tcf/eclipse/plugins/?d) | 
| 12 | eclipse/plugins/javaewah\_0.7.9.v201401101600.jar | dev-java/javaewah | 0.7.9 |  |  | [Sources](https://github.com/lemire/javaewah/releases?after=JavaEWAH-0.8.0) | 
| 13 | eclipse/plugins/javax.annotation\_1.2.0.v201401042248.jar | dev-java/jsr250 | 1.2 | [eclipse](https://github.com/gentoo/eclipse-overlay/tree/master/dev-java/jsr250) | gentoo: 1.0 | [JSR 250](https://www.jcp.org/en/jsr/detail?id=250) | 
| 14 | eclipse/plugins/javax.el\_2.2.0.v201303151357.jar | dev-java/commons-el(?) | 2.2.0 |  | gentoo: 1.0 | [JSR 245](http://download.oracle.com/otndocs/jcp/expression_language-2.2-mrel-eval-oth-JSpec/) | 
| 15 | eclipse/plugins/javax.inject\_1.0.0.v20091030.jar | dev-java/javax-inject | 1.0.0 | gentoo |  | [JSR-330](https://www.jcp.org/en/jsr/detail?id=330) | 
| 16 | eclipse/plugins/javax.servlet\_3.1.0.v201410161800.jar | java-virtuals/servlet-api | 3.1 |  | gentoo: 3.0 | [JSR 315](https://www.jcp.org/en/jsr/detail?id=315) [Source .jar](https://search.maven.org/remotecontent?filepath=javax/servlet/javax.servlet-api/3.1.0/javax.servlet-api-3.1.0-sources.jar) | 
| 17 | eclipse/plugins/javax.servlet.jsp\_2.2.0.v201112011158.jar | dev-java/jsp-api | 2.2 |  |  | [JSR 245](https://www.jcp.org/en/jsr/detail?id=245) [Source .jar](https://search.maven.org/remotecontent?filepath=javax/servlet/jsp/jsp-api/2.2/jsp-api-2.2-sources.jar) | 
| 18 | eclipse/plugins/javax.xml\_1.3.4.v201005080400.jar | dev-java/xml-commons(?) | 1.3.4 |  | gentoo: 1.0\_beta2 | [JSR 206](https://jcp.org/en/jsr/detail?id=206) | 
| 19 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-antlr.jar | dev-java/ant-antlr | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 20 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-apache-bcel.jar | dev-java/ant-apache-bcel | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 21 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-apache-bsf.jar | dev-java/ant-apache-bsf | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 22 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-apache-log4j.jar | dev-java/ant-apache-log4j | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 23 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-apache-oro.jar | dev-java/ant-apache-oro | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 24 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-apache-regexp.jar | dev-java/ant-apache-regexp | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 25 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-apache-resolver.jar | dev-java/ant-apache-resolver | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 26 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-apache-xalan2.jar | dev-java/ant-apache-xalan2 | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 27 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-commons-logging.jar | dev-java/ant-commons-logging | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 28 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-commons-net.jar | dev-java/ant-commons-net | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 29 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-jai.jar | dev-java/ant-jai | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 30 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant.jar | dev-java/ant-core | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 31 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-javamail.jar | dev-java/ant-javamail | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 32 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-jdepend.jar | dev-java/ant-jdepend | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 33 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-jmf.jar | dev-java/ant-jmf | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 34 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-jsch.jar | dev-java/ant-jsch | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 35 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-junit4.jar | dev-java/ant-junit4 | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 36 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-junit.jar | dev-java/ant-junit | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 30 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-launcher.jar | dev-java/ant-core | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 38 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-netrexx.jar | dev-java/ant-netrexx | 1.9.4 |  |  | [Sources](https://repo1.maven.org/maven2/org/apache/ant/ant-netrexx/1.9.4/) | 
| 39 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-swing.jar | dev-java/ant-swing | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 40 | eclipse/plugins/org.apache.ant\_1.9.4.v201504302020/lib/ant-testutil.jar | dev-java/ant-testutil | 1.9.4 |  | gentoo: 1.9.2 |  | 
| 41 | eclipse/plugins/org.apache.batik.css\_1.7.0.v201011041433.jar | dev-java/batik | 1.7 |  | gentoo: 1.8 |  | 
| 42 | eclipse/plugins/org.apache.batik.util\_1.7.0.v201011041433.jar | dev-java/batik | 1.7 |  | gentoo: 1.8 |  | 
| 43 | eclipse/plugins/org.apache.batik.util.gui\_1.7.0.v200903091627.jar | dev-java/batik | 1.7 |  | gentoo: 1.8 |  | 
| 44 | eclipse/plugins/org.apache.commons.codec\_1.6.0.v201305230611.jar | dev-java/commons-codec | 1.6.0 |  | gentoo: 1.7 |  | 
| 45 | eclipse/plugins/org.apache.commons.compress\_1.6.0.v201310281400.jar | dev-java/commons-compress | 1.6.0 |  | gentoo: 1.4.1, 1.8.1 |  | 
| 46 | eclipse/plugins/org.apache.commons.httpclient\_3.1.0.v201012070820.jar | dev-java/commons-httpclient | 3.1.0 | gentoo |  |  | 
| 47 | eclipse/plugins/org.apache.commons.io\_2.2.0.v201405211200.jar | dev-java/commons-io | 2.2 | [eclipse](https://github.com/gentoo/eclipse-overlay/tree/master/dev-java/commons-io) | gentoo: 2.0.1, 2.4 |  | 
| 48 | eclipse/plugins/org.apache.commons.jxpath\_1.3.0.v200911051830.jar | dev-java/commons-jxpath | 1.3.0 | gentoo |  |  | 
| 49 | eclipse/plugins/org.apache.commons.lang\_2.6.0.v201404270220.jar | dev-java/commons-lang | 2.6.0 | gentoo |  |  | 
| 50 | eclipse/plugins/org.apache.commons.lang3\_3.1.0.v201403281430.jar | dev-java/commons-lang | 3.1 | gentoo |  |  | 
| 51 | eclipse/plugins/org.apache.commons.logging\_1.1.1.v201101211721.jar | dev-java/commons-logging | 1.1.1 | gentoo |  |  | 
| 52 | eclipse/plugins/org.apache.commons.math\_2.1.0.v201105210652.jar | dev-java/commons-math | 2.1.0 | gentoo |  |  | 
| 53 | eclipse/plugins/org.apache.commons.pool\_1.6.0.v201204271246.jar | dev-java/commons-pool | 1.6 | gentoo |  |  | 
| 54 | eclipse/plugins/org.apache.felix.gogo.command\_0.10.0.v201209301215.jar | dev-java/felix-gogo-command | 0.10.0 |  | gentoo: 0.12.0 |  | 
| 55 | eclipse/plugins/org.apache.felix.gogo.runtime\_0.10.0.v201209301036.jar | dev-java/felix-gogo-runtime | 0.10.0 | gentoo |  |  | 
| 56 | eclipse/plugins/org.apache.felix.gogo.shell\_0.10.0.v201212101605.jar | dev-java/felix-gogo-shell | 0.10 |  |  | [Source .tar.gz](http://mirrors.ae-online.de/apache//felix/org.apache.felix.gogo.shell-0.10.0-project.tar.gz) | 
| 57 | eclipse/plugins/org.apache.httpcomponents.httpclient\_4.3.6.v201411290715.jar | dev-java/httpcomponents-client | 4.3.6 |  | gentoo: 4.5 |  | 
| 58 | eclipse/plugins/org.apache.httpcomponents.httpcore\_4.3.3.v201411290715.jar | dev-java/httpcomponents-core | 4.3.3 |  | gentoo: 4.4.1 |  | 
| 59 | eclipse/plugins/org.apache.jasper.glassfish\_2.2.2.v201501141630.jar | dev-java/glassfish-jsp-api | 2.2.2 |  |  | [Source .jar](https://search.maven.org/remotecontent?filepath=org/eclipse/jetty/orbit/org.apache.jasper.glassfish/2.2.2.v201112011158/org.apache.jasper.glassfish-2.2.2.v201112011158-sources.jar) | 
| 60 | eclipse/plugins/org.apache.log4j\_1.2.15.v201012070815.jar | dev-java/log4j | 1.2.15 | [eclipse](https://github.com/gentoo/eclipse-overlay/tree/master/dev-java/log4j) | gentoo: 1.2.16, 1.2.17 |  | 
| 61 | eclipse/plugins/org.apache.lucene.analysis\_3.5.0.v20120725-1805.jar | dev-java/lucene | 3.5 | gentoo |  |  | 
| 62 | eclipse/plugins/org.apache.lucene.core\_3.5.0.v20120725-1805.jar | dev-java/lucene | 3.5 | gentoo |  |  | 
| 63 | eclipse/plugins/org.apache.solr.client.solrj\_3.5.0.v20150506-0844.jar | dev-java/apache-solr | 3.5 |  | [ultrabug](http://gpo.zugaina.org/dev-db/apache-solr): 3.6 [tmacedo](http://gpo.zugaina.org/dev-db/apache-solr): 3.6.2 | [3.5 sources](https://archive.apache.org/dist/lucene/solr/3.5.0/) | 
| 64 | eclipse/plugins/org.apache.ws.commons.util\_1.0.1.v20100518-1140.jar | dev-java/ws-commons-util | 1.0.1 | gentoo |  |  | 
| 65 | eclipse/plugins/org.apache.xerces\_2.9.0.v201101211617.jar | dev-java/xerces | 2.9.0 |  | gentoo: 2.11 |  | 
| 66 | eclipse/plugins/org.apache.xml.resolver\_1.2.0.v201005080400.jar | dev-java/xml-commons-resolver | 1.2.0 | gentoo |  |  | 
| 67 | eclipse/plugins/org.apache.xmlrpc\_3.0.0.v20100427-1100.jar | dev-java/xmlrpc | 3.0.0 |  | gentoo: 3.1.3 |  | 
| 68 | eclipse/plugins/org.apache.xml.serializer\_2.7.1.v201005080400.jar | dev-java/xalan-serializer | 2.7.1 | gentoo |  |  | 
| 69 | eclipse/plugins/org.hamcrest.core\_1.3.0.v201303031735.jar | dev-java/hamcrest-core | 1.3.0 | gentoo |  |  | 
| 70 | eclipse/plugins/org.jsoup\_1.7.2.v201411291515.jar | dev-java/jsoup | 1.7.2 | gentoo |  |  | 
| 71 | eclipse/plugins/org.junit\_4.12.0.v201504281640/junit.jar | dev-java/junit | 4.12 | gentoo |  |  | 
| 72 | eclipse/plugins/org.sat4j.core\_2.3.5.v201308161310.jar | dev-java/sat4j-core | 2.3.5 |  | gentoo: 2.3.1 |  | 
| 73 | eclipse/plugins/org.sat4j.pb\_2.3.5.v201404071733.jar | dev-java/sat4j-pseudo | 2.3.5 |  | gentoo: 2.3.1 |  | 
| 74 | eclipse/plugins/org.slf4j.api\_1.7.2.v20121108-1250.jar | dev-java/slf4j-api | 1.7.2 |  | gentoo: .. 1.6.6 1.7.5 .. |  | 
| 75 | eclipse/plugins/org.slf4j.impl.log4j12\_1.7.2.v20131105-2200.jar | dev-java/slf4j-log4j12 | 1.7.2 |  | gentoo: 1.7.5 |  | 
| 76 | eclipse/plugins/org.tukaani.xz\_1.3.0.v201308270617.jar | dev-java/xz-java | 1.3 | [eclipse](https://github.com/gentoo/eclipse-overlay/tree/master/dev-java/xz-java) | gentoo: 1.4, 1.5 |  | 
| 77 | eclipse/plugins/org.w3c.css.sac\_1.3.1.v200903091627.jar | dev-java/sac | 1.3.1 |  | gentoo: 1.3 |  | 
| 78 | eclipse/plugins/org.w3c.dom.events\_3.0.0.draft20060413\_v201105210656.jar | dev-java/w3c-dom-events | 3.0.0 | [eclipse](https://github.com/gentoo/eclipse-overlay/tree/master/dev-java/w3c-dom-events) |  |  | 
| 79 | eclipse/plugins/org.w3c.dom.smil\_1.0.1.v200903091627.jar | dev-java/w3c-dom-smil | 1.0.1 | [eclipse](https://github.com/gentoo/eclipse-overlay/tree/master/dev-java/w3c-dom-smil) |  |  | 
| 80 | eclipse/plugins/org.w3c.dom.svg\_1.1.0.v201011041433.jar | dev-java/w3c-dom-svg | 1.1.0 | [eclipse](https://github.com/gentoo/eclipse-overlay/tree/master/dev-java/w3c-dom-svg) |  |  |
