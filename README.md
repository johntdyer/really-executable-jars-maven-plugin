To use it, add a plugin to your pom like

``` xml

<!-- You need to build an exectuable uberjar, I like Shade for that -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>2.0</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">

                        <!-- note that the main class is set *here* -->

                        <mainClass>com.example.Main</mainClass>
                    </transformer>
                </transformers>
                <createDependencyReducedPom>false</createDependencyReducedPom>
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
        </execution>
    </executions>
</plugin>

<!-- now make the jar chmod +x style executable -->
<plugin>
  <groupId>org.skife.maven</groupId>
  <artifactId>really-executable-jar-maven-plugin</artifactId>
  <version>1.1.0</version>
  <configuration>
    <!-- value of flags will be interpolated into the java invocation -->
    <!-- as "java $flags -jar ..." -->
    <flags>-Xmx1G</flags>

    <!-- (optional) name for binary executable, if not set will just -->
    <!-- make the regular jar artifact executable -->
    <programFile>nifty-executable</programFile>
  </configuration>

  <executions>
    <execution>
      <phase>package</phase>
      <goals>
        <goal>really-executable-jar</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

Changes:

1.1.0 - If programFile is set, do not make the base artifact (the
.jar) executable, just the programFile one.

1.0.0 - Stable
