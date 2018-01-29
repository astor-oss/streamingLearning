# SQL工具
1. calcite
    - slideShare: https://www.slideshare.net/JordanHalterman/introduction-to-apache-calcite?next_slideshow=1
    - slideShared: https://www.slideshare.net/julianhyde/streaming-sql-with-apache-calcite
2. Auto gen code
    - io.swagger-codegen-maven-plugin(Ref: athenax)
``` java demo
    <sourceDirectory>src/main/java</sourceDirectory>
    <plugins>
      <plugin>
        <groupId>io.swagger</groupId>
        <artifactId>swagger-codegen-maven-plugin</artifactId>
        <executions>
          <execution>
            <goals>
              <goal>generate</goal>
            </goals>
            <configuration>
              <inputSpec>${project.basedir}/src/main/resources/athenax-backend-api.yaml</inputSpec>
              <language>jaxrs</language>
              <output>target/swagger</output>
              <modelPackage>com.uber.athenax.backend.api</modelPackage>
              <apiPackage>com.uber.athenax.backend.api</apiPackage>
              <invokerPackage>com.uber.athenax.backend.api</invokerPackage>
              <ignoreFileOverride>${project.basedir}/src/main/resources/.swagger-codegen-ignore</ignoreFileOverride>
              <supportingFilesToGenerate>NotFoundException.java,ApiException.java,ApiResponseMessage.java</supportingFilesToGenerate>
              <configOptions>
                <dateLibrary>java8</dateLibrary>
                <library>jersey1</library>
              </configOptions>
              <addCompileSourceRoot>false</addCompileSourceRoot>
            </configuration>
          </execution>
        </executions>
      </plugin>
```

    - googlecode.fmpp-maven-plugin
