1. Write a build script to generate 3 different flavors (variations) of the artifact (jar, war, ear) for the same code base with the only change of configuration files amongst them (properties, xml , txt ..etc) using any  build tool (ANT ,Maven , Gradle). 

i have used maven for the above task.
Assuming a maven code base having configuration files in the format of properties file ,txt and xml.

POM.xml:
the configuration used to generate the jar file.

POM1.xml
the configuration used to generate the war file.

POM2.xml
the configuration used to generate the ear file
 assuming a maven project with mutiple modules which generates jar and war files.
 
