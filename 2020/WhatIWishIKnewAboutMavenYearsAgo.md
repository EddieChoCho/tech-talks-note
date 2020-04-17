# What I Wish I Knew About Maven Years Ago
### Speaker: Andres Almiray
2020/04/15

## Override Project Properties
mvn -Pkey = value
* Properties may be defined
	* In a <properties> block
	* By plugins
* Property values defined on the command line have precedence

* Must specify the Java version
	* Compiler plugin as a full block in your pom, and specify two properties: compiler source and compler target
	* Or define those properties inside the properties block.

## Invoke plugins on the go
* mvn G:A:V:goal
* you can invoke any plugin in a project without having to configure the plugin in the pom file.
* Just pass in the maven coordinates/gap coordinates for the plugin
* Any plugin may be applied using the long notation
G:A:V:goal, in other words
mvn. om.github.ekryd.echo-maven-plugin:echo-maven-plugin:1.2.0:echo -Decho.message="Hello Wrold!"
* First time invoke the plugin, maven will download the plugin before execute it.

## Dependency Resolution
* Dependencies
	* Dependencies defined in a <dependencies> block are know as direct dependencies.
	* Dependencies brought by direct dependencies are known as transitive dependencies.
	* Maven resolves dependencies by locality
		- direct in the current POM. Las found wins (closest).
		- direct in the parent hierarchy.
		- transitive in the hierarchy based on number of hopes to reach it. First found wins.
	* You can check the dependencies with: mvn dependnecny: tree

* Bobert Scholte: Maven never looks to the version, but always to the location in the tree. With the huge dependency trees nowadays i think we should reconsider this in a future major release of Maven. There are enforcer rules to protect you.
* The solution in the meantime
	* Apply maven enforcer plug-in - It will let you know if there is a conflict. It can also force specific versions.
		* If there is a conflict, the build will failed fast which help you to find the issue in the earlier stage.
	* Apply the dependencyManagement block - The definition is saying that I want to consum a version of one. No matter where the maven is going to find any dependency graph. But if it does, it better be the version which set in the dependencyManagement block

## Do not mvn clean install - Should invoke mvn verify instead.
* Mvn verify
	* The Reactor knows how to build all requirements.
	* The install goal should only be used when artifacts must be published to the local repository.
	* Invoking clean diminishes the reuse of intermediate results -> defeats incremental builds.
* Invoking a subset of maven reactor
	* In Gradle you can invoke any goal on any project and it will build all the dependencies that are needed, you only need to specify that particular goal not everything else.
	* In maven we have something similar: mvn -am -pl (am: also make, pl: project list)
	
* Partial Execution 
	* Maven runs the multi-project builds inside a Reactor.
	* The Reactor executes goals for all projects.
	* mvn -pl <paths> executes the given goals for all projects in <path>
	* mvn -am -pl <paths> also executes for all prerequisites in the Reactor.
	
* Restrictions
	* All invoked plugin goals must exist in every project!
	* Define common plugins in parent POMs using a combination of <pluginManagement> and <plugins>
	* You'd be forced to define dummy values for plugins in some cases.

## Aggregating POMs
* These POM files define a <modules> section.
* They are typically also parent POMs but they DO NOT have to be!
* Also, values in <module> are paths, not project names.


## Further readings
* [Surviving Dependency Hell Maven](https://saturnim.me/talk/surviving-dependency-hell-maven)
* [Broken Buildtools and Bad Behaviors; The Maven Story by Robert Scholte](https://www.youtube.com/watch?v=2HyGxtsDf60)
* [Test Your Knowledge about Maven](http://andresalmiray.com/maven-dependencies-pop-quiz-results/)
