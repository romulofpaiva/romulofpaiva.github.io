### JAXB2 Maven Plugin

[JAXB2 Maven Plugin](https://www.mojohaus.org/jaxb2-maven-plugin/Documentation/v2.4/index.html) plugin allows generation of Java classes from XML schemas and XML schemas from Java classes.

#### Java classes generation from schema definition (WSDL)

1. Put the schema definition (WSDL) in the src/main/resources/wsdl folder of your project.

2. Include in your project's pom.xml the configuration for JAXB2 Maven Plugin.

3. Compile your project.

```
...
<build>
   <plugins>
      <plugin>
         <groupId>org.codehaus.mojo</groupId>
         <artifactId>jaxb-maven-plugin</artifactId>
         <version>2.5.0</version>
         <executions>
            <execution>
               <id>xjc></id>
               <goals>
                  <goal>xjc</goal>
               </goals>
            </execution>
         </executions>
         <configuration>
            <outputDirectory>${project.baseDir}/src/main/java</outputDirectory>
            <packageName>com.example.mypackage</packageName>  <!--Enter here the package name where you want to generate the Java classes-->
            <sourceType>wsdl</sourceType>
            <clearOutputdir>false</clearOutputdir>
            <noPackageLevelAnnotations>true</noPackageLevelAnnotations>
            <sources>
               <source>${project.baseDir}/src/main/resources/wsdl</sources>
            </sources>
         </configuration>
      </plugin>
   </plugins>
</build>
...
``` 

#### Bean definition

TODO

#### Interface definition

TODO

