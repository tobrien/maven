Description
===========

Install and configure maven2 and maven3 from the binaries provided by
the maven project

Has `maven` LWRP for pulling a maven artifact from a maven repository and
placing it in an arbitrary location.

Has `maven_repo` LWRP that allows you to add your custom repo to the list
of repos searched for artifacts

Requirements
============

Platform:

* Debian, Ubuntu, CentOS, Red Hat, Fedora

The following Opscode cookbooks are dependencies:

* java - this cookbook not only depends on the java virtual machine
  but it also depends on the java_ark LWRP present in the java cookbooks
* ark - used to unpack the maven tarball

Attributes
==========

* default['maven']['version']  defaults to 2
* default['maven']['m2_home']  defaults to  '/usr/local/maven/'
* default['maven']['m2_download_url']  the download url for maven2
* default['maven']['m2_checksum']  the checksum, which you will have
 to recalculate if you change the download url
* default['maven']['m3_download_url'] download url for maven3
* default['maven']['m3_checksum'] the checksum, which you will have
 to recalculate if you change the download url


Usage
=====

Simply include the recipe where you want Apache Maven installed.


Providers/Resources
===================

maven
-----

* artifact_id: if this is not specified, the resource's name is used
* group_id: group_id for the artifact
* version: version of the artifact
* dest: the destination folder for the jar and its dependencies
* packaging: defaults to 'jar'
* repositories: array of maven repositories to use, defaults to
 ["http://repo1.maven.apache.org/maven2"]
* owner: the owner of the resulting file, default is root
* mode: integer value for file permissions, default is 0644


# Examples

 The following invocation of the Maven LWRP will retrieve the
 mysql-connector-java-5.1.19.jar file and put this file into
 /usr/local/tomcat/lib.  
 
    maven "mysql-connector-java" do
      group_id "mysql"
      version "5.1.19"
      dest "/usr/local/tomcat/lib/"
    end
    
The previous invocation will download all the dependencies of
 mysql-connector-java to a local Maven repository, and then copy a
 single file to /usr/local/tomcat/lib.   If you want to avoid
 downloading all of the transitive dependencies (maybe because you are
 downloading a hefty WAR and you don't want to wait for dependencies),
 specify "false" for
 transitive.
 
    maven "spring-webmvc" do
      group_id "org.springframework"
      version "3.1.1.RELEASE"
      dest "/usr/local/tomcat/lib/"
      transitive "false"
    end

If you need to specify the destination filename, use the dest_file
parameter.  If dest_file is not specified, this LWRP will assume that
you want the artifact to be saved with the standard format
artifactId-version.packaging. If you specify dest_file you can
override that.  This comes in handy if you are downloading something
like a web application (WAR file) and you want to save the file under
a name that doesn't include the version.  Here's an example with the
Apache Solr WAR file.  (Note that this example depends on the tomcat6
cookbook and saves a WAR file to the webapps directory of Tomcat).

    maven "solr" do
      group_id "org.apache.solr"
      version "3.6.0"
      packaging "war"
      dest "#{node[:tomcat6][:webapps]}"
      dest_file "solr.war"
      transitive "false"
    end

maven_repo
----------

TODO, coming soon

License and Author
==================

Author:: Seth Chisamore (<schisamo@opscode.com>)
Author:: Bryan W. Berry (<bryan.berry@gmail.com>)
Author:: Tim O'Brien (<tobrien@discursive.com>)

Copyright 2010-2012, Opscode, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
