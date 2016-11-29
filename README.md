gradle-mvn-push
===============

Forked from https://github.com/chrisbanes/gradle-mvn-push

See Chris Banes' blog post for more context on this 'library': [http://chris.banes.me/2013/08/27/pushing-aars-to-maven-central/](http://chris.banes.me/2013/08/27/pushing-aars-to-maven-central/).


# Configure your project

### 1. Code up your library
Get a library project up and running.


### 2. Create project root gradle.properties
Make a gradle.properties file in your project's root directory. If the file already exists just add the missing fields.

```groovy
VERSION_NAME=1.0.0
//When pushing to oss.sonatype.org append '-SNAPSHOT' to the version name
//to publish to maven's snapshot repo.
//Snapshot for jCenter is not supported for now.
VERSION_CODE=2
GROUP=com.mycompany.myproduct
RELEASE_SOURCE=false

POM_DESCRIPTION=A short description of this library
POM_URL=https://github.com/organization/My-Library
POM_SCM_URL=https://github.com/organization/My-Library
POM_SCM_CONNECTION=scm:git@github.com:organization/My-Library.git
POM_SCM_DEV_CONNECTION=scm:git@github.com:organization/My-Library.git
POM_GIT_URL=https://github.com/organization/My-Library.git
POM_LICENCE_NAME=The Apache Software License, Version 2.0
POM_LICENCE_URL=http://www.apache.org/licenses/LICENSE-2.0.txt
POM_LICENCE_DIST=repo
POM_DEVELOPER_ID=organization
POM_DEVELOPER_NAME=Organization
```

### 2. Create project root gradle.properties
Create gradle.properties in each sub-project
The values in this file are specific to the sub-project (and override those in the root `gradle.properties`). In this example, this is just the name, artifactId and packaging type:

```properties
POM_NAME=My awesome library
POM_ARTIFACT_ID=MyLibrary
POM_PACKAGING=aar
```

### 3. Update your home gradle.properties

This will include the username and password to upload to the Maven server and so that they are kept local on your machine. The location defaults to `USER_HOME/.gradle/gradle.properties`.

It may also include your signing key id, password, and secret key ring file (for signed uploads).  Signing is only necessary if you're putting release builds of your project.

```properties
NEXUS_USERNAME=username
NEXUS_PASSWORD=p@$$\^/0rd
```

#### Maven central (hosted by oss.sonatype.org)
Extra properties needed only for maven central

```properties
signing.keyId=myid
signing.password=Y0urP@$$\^/0rd
signing.secretKeyRingFile=~/.gnupg/secring.gpg
```

#### jCenter (hosted ny bintray.com)
Extra properties needed only for bintray

```properties
BINTRAY_USER=username
BINTRAY_API_KEY=y0urapikeyp1eas3
BINTRAY_GPG_PASSWORD=p@$$\^/0rd
```

### 5. Update each sub-module's `build.gradle`

Add the following at the end of each `build.gradle` that you wish to upload:

```groovy
apply from: 'https://raw.githubusercontent.com/k1slay/gradle-mvn-push/master/gradle-mvn-push.gradle'
```

### 6. Update root `build.gradle` (only for jCenter)

Skip if only publishing to mavenCentral

Add these lines in dependencies

```groovy
dependencies {
    ...
    classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
    classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'
}
```

### 7. Build and Push

You can now build and push:

#### Maven central

```bash
$ gradle clean build uploadArchives
```

#### jCenter 

```bash
$ gradle clean build bintrayUpload
```

## License

    Copyright 2013 Chris Banes

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
