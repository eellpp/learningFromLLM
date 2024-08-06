There are several useful `mvn` (Maven) commands that you should be aware of as a Java developer working with Maven. Here are some of the most commonly used ones:

### Build Lifecycle Commands
1. **`mvn clean`**:
   - Deletes the `target` directory, which contains the compiled artifacts and other build output.

2. **`mvn compile`**:
   - Compiles the source code of the project.

3. **`mvn test`**:
   - Runs the tests using a suitable unit testing framework.

4. **`mvn package`**:
   - Compiles the code, runs tests, and packages the code into a distributable format, such as a JAR or WAR file.

5. **`mvn install`**:
   - Installs the packaged project into the local repository, which can be used as a dependency in other projects locally.

6. **`mvn deploy`**:
   - Deploys the packaged code to a remote repository for sharing with other developers and projects.

### Dependency Management
1. **`mvn dependency:tree`**:
   - Prints the dependency tree for the project, showing how dependencies are related.

2. **`mvn dependency:analyze`**:
   - Analyzes the project's dependencies and reports on unused declared dependencies and used undeclared dependencies.

3. **`mvn dependency:purge-local-repository`**:
   - Purges specific artifacts and their dependencies from the local repository, forcing them to be re-downloaded.

### Project Information
1. **`mvn validate`**:
   - Validates the project is correct and all necessary information is available.

2. **`mvn site`**:
   - Generates a site for the project, including reports like Javadoc and Checkstyle.

3. **`mvn versions:display-dependency-updates`**:
   - Displays the available updates for the dependencies in the project.

4. **`mvn versions:display-plugin-updates`**:
   - Displays the available updates for the plugins used in the project.

### Plugin and Execution
1. **`mvn exec:java`**:
   - Executes a Java program within the project. Requires specifying the main class to run.

2. **`mvn exec:exec`**:
   - Executes any arbitrary program or script within the context of the project.

### Cleaning and Tidying
1. **`mvn clean:clean`**:
   - Cleans up after the build by removing files generated during the build process.

2. **`mvn clean:dependency-clean`**:
   - Cleans dependencies in the project.

### Running Specific Goals or Phases
1. **`mvn help:describe -Dcmd=command`**:
   - Describes a particular Maven command in detail, useful for understanding what a specific command does.

2. **`mvn help:effective-pom`**:
   - Displays the effective POM as an XML for the current build, showing inherited and merged configurations.

These commands cover a wide range of tasks that you might need to perform when working with Maven, from building and testing your code to managing dependencies and generating project documentation.

## Using the command  mvn::list

The command `mvn dependency:list` is used in Apache Maven, a build automation tool for Java projects. This command lists the dependencies of a Maven project.

When you run `mvn dependency:list`, Maven will:

1. **Resolve Dependencies**: Resolve the project's dependencies, including transitive dependencies (those required by your project's direct dependencies).
2. **Display Dependencies**: Print a list of all resolved dependencies to the console, including their group ID, artifact ID, version, scope, and more.

Here is an example of how to use it:

```bash
mvn dependency:list
```

This command is particularly useful for:

- **Reviewing Dependencies**: Ensuring that all required dependencies are included and correctly specified.
- **Troubleshooting**: Identifying conflicts or unexpected dependencies.
- **Documentation**: Generating a list of dependencies for documentation or auditing purposes.

You can customize the output using additional options. For example, you can filter the dependencies to only show certain types or formats. For detailed usage, you can refer to the [Maven Dependency Plugin documentation](https://maven.apache.org/plugins/maven-dependency-plugin/list-mojo.html).
