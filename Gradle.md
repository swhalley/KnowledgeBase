
# Include Files in Jar and on classpath
You may know how to use `compile 'com.google.code.gson:gson:2.8.2'` to pull a resource from the maven repository. But to include custom files in a folder add the following

```build.gradle
dependencies {
   compile fileTree( dir: 'resources/libs', include: ['*.jar'] )
}
```
This will pull the files into your resulting war/jar/ear (or whatever) and make sure that these files are on the classpath.

# Distribution Package
A lot of the plugins for gradle (application, jar and more) extend the distribution plugin. The distribution plugin allows a space for you to put files which should be exposed outside of your war/jar/ear. An example of this may be a certificate or a configuration file.

The Distribution package allows you to create a special folder where these files can live.
create the folder `src/dist/` where these files can live

The difference between `src/dist` and `src/main/resources` is that the latter is compiled/compressed into the jar/ear/war where the first is exposed and available for the user to freely change. This is what makes this a good choice for property files which may change frequently. example Log4j.properties

# Manage a variable for Prod and Development
There are times where you may have to have a path specific for development purposes and a different for production code. One use case for this may be to pass parameters into the application.

Here is an example. I create a custom trust store. As part of destribution package, I put the trustStore file under `src/dist/certificate/info`. Using `gradle run` and `gradle build` offers two different experiences.
 * `gradle run` compiles the java source and runs it.
    * The trust store is not on the compile path so it is ignored for this operation
 * `gradle build` compiles the code, copies files around, and creates a package I can distribute. 
    * The build operation pulls in the trust store and packages it for distribution
    
 Because of this small difference, development requires a different path to the file than distribution
 * Development Path - src/dist/certificate/info/customTrustStore
 * Distribution Path - ../certificate/info/customTrustStore
    * This path may change according to the final packaging of the application. but `src/dist` will always get dropped
    
 To solve this issue, the best way found was to extend the `run` task to ensure the two paths can be managed. Here is an example of the `gradle.config` file using the application plugin
 ```gradle.config
 //Default to distribution location
 applicationDefaultJvmArgs = ["-Djavax.net.ssl.trustStore=../certificate/info/customTrustStore"]
 
 //If using the run command then we change it
 run {
    doFirst {
       applicationDefaultJvmArgs = ["-Djavax.net.ssl.trustStore=src/dist/certificate/info/customTrustStore"]
    }
 }
 ```
 
 # See dependencies of a task
 `gradle taskName --dry-run`
