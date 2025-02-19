## Open rewrite plugin  

Written a custom open rewrite plugin that adds a custom maven profile to a java project.  

### To run locally

#### Approach 1 : Update pom configuration  

1. Use below plugin setup in pom.xml
```xml
<project>
    <build>
        <plugins>
            <plugin>
                <groupId>org.openrewrite.maven</groupId>
                <artifactId>rewrite-maven-plugin</artifactId>
                <version>6.1.3</version>
                <configuration>
                    <exportDatatables>true</exportDatatables>
                    <activeRecipes>
                        <recipe>com.prazjain.openrewrite.maven.AddUnitTestMavenProfile</recipe>
                    </activeRecipes>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>com.prazjain</groupId>
                        <artifactId>openrewrite-custom-mvn-profile</artifactId>
                        <version>1.0-SNAPSHOT</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
</project>
```  

2. Run `mvn rewrite:run` to run the recipe  


#### Approach 2 : No changes to pom, just give params on the command line  

This is better approach when you want your client projects to run this recipe just once, and not have it in their build process.  

```commandline
mvn -U org.openrewrite.maven:rewrite-maven-plugin:6.1.3:run   -Drewrite.recipeArtifactCoordinates=com.prazjain:openrewrite-custom-mvn-profile:1.0-SNAPSHOT   -Drewrite.activeRecipes=com.prazjain.openrewrite.maven.AddUnitTestMavenProfile  
```

Readme page here shows other ways to include and run your recipe once they are published https://github.com/openrewrite/rewrite-maven-plugin  

### Result  

You will see your client project's pom updated to include below profile information  
```xml
    <profiles>
        <profile>
            <id>unit-tests</id>
            <activation>
                <foo>foo</foo>
            </activation>
            <properties>
                <foo>foo</foo>
            </properties>
            <build>
                <foo>foo</foo>
            </build>
        </profile>
    </profiles>
```