# Versioning



Simple Tag
`git tag -a "v1.0.0"`

Tag with Explanation
`git tag -a "v1.0.0" -m "Something about the tag"`

Latest Version
`git describe`

If the last commit was tagged, you get the `-a` version back. If you have commited since the last tag, you get the `-a` plus the commit hash.

# Adding version to create-react-app 

*package.json*
```javascript
"scripts" :  {
  "build" : "REACT_APP_VERSION=$(git describe) react-scripts build"
}
```

*index.html*
```html
<meta name="version" content="%REACT_APP_VERSION%">
```


# Adding version to gradle build
https://docs.gradle.org/current/userguide/tutorial_java_projects.html
https://ryanharter.com/blog/2013/07/30/automatic-versioning-with-git-and-gradle/

*build.gradle*
```java
apply plugin: 'java'
apply plugin: 'eclipse'

def getVersion = { ->
    def stdout = new ByteArrayOutputStream()
    exec {
        commandLine 'git', 'describe', '--tags'
        standardOutput = stdout
    }
    return stdout.toString().trim()
}

version = getVersion()

jar {
    manifest {
        attributes 'Implementation-Title': 'Gradle Quickstart',
                   'Implementation-Version': version
    }
}

```

Additionally `git describe --tags --abbrev=0` will give the tag without the commit hash

# Remove Cached Files
`git rm -r --cached .`
