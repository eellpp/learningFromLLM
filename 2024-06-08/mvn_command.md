
 There are several useful `mvn` (Maven) commands that you should be aware of as a Java developer working with Maven. Here are some of the most commonly used ones:


 Here's a table of essential `mvn` (Maven) commands formatted for easy copying and printing:

| **Category**             | **Command**                         | **Description**                                                              |
|--------------------------|-------------------------------------|------------------------------------------------------------------------------|
| **Build Lifecycle**      | `mvn clean`                         | Deletes the `target` directory.                                              |
|                          | `mvn compile`                       | Compiles the source code of the project.                                     |
|                          | `mvn test`                          | Runs the tests using a suitable unit testing framework.                      |
|                          | `mvn package`                       | Compiles code, runs tests, and packages the code into a JAR/WAR file.        |
|                          | `mvn install`                       | Installs the packaged project into the local repository.                     |
|                          | `mvn deploy`                        | Deploys the packaged code to a remote repository.                            |
| **Dependency Management**| `mvn dependency:list`               | Lists the dependencies of a project.                                         |
|                          | `mvn dependency:tree`               | Prints the dependency tree for the project.                                  |
|                          | `mvn dependency:analyze`            | Reports on unused declared dependencies and used undeclared dependencies.    |
|                          | `mvn dependency:purge-local-repository` | Purges specific artifacts and their dependencies from the local repository.  |
| **Project Information**  | `mvn validate`                      | Validates the project configuration.                                         |
|                          | `mvn site`                          | Generates a site for the project, including reports like Javadoc and Checkstyle. |
|                          | `mvn versions:display-dependency-updates` | Displays the available updates for the dependencies in the project.       |
|                          | `mvn versions:display-plugin-updates` | Displays the available updates for the plugins used in the project.       |
| **Plugin and Execution** | `mvn exec:java`                     | Executes a Java program within the project.                                  |
|                          | `mvn exec:exec`                     | Executes any arbitrary program or script within the context of the project.  |
| **Cleaning and Tidying** | `mvn clean:clean`                   | Cleans up files generated during the build process.                          |
|                          | `mvn clean:dependency-clean`        | Cleans dependencies in the project.                                          |
| **Running Specific Goals or Phases** | `mvn help:describe -Dcmd=command` | Describes a particular Maven command in detail.                          |
|                          | `mvn help:effective-pom`            | Displays the effective POM as an XML for the current build.                  |




## Using the command  mvn::list

The command `mvn dependency:list` is used in Apache Maven, a build automation tool for Java projects. This command lists the dependencies of a Maven project.

When you run `mvn dependency:list`, Maven will:

1. **Resolve Dependencies**: Resolve the project's dependencies, including transitive dependencies (those required by your project's direct dependencies).
2. **Display Dependencies**: Print a list of all resolved dependencies to the console, including their group ID, artifact ID, version, scope, and more.

Here is an example of how to use it:

