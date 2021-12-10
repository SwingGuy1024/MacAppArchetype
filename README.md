# Mac Application Maven Archetype

This is a maven archetype to create a bundled stand-alone application for the Macintosh.

## Build

### Before building
1. You may need to go into `Info.plist` and change ${mainClass} to the fully qualified class name of the main class. This can be done by Maven with a script, but I don't know how to do that right now.
2. Replace the empty `appIcon.icns` file with a real Icons file.
3. You must use JDK 8 or 9. The app bundler doesn't work with later versions of the JDK.


### Building
To build do the following:

    mvn clean install
    mvn install archetype:update-local-catalog

I'm not sure if the second line is necessary. Some people will say that the clean is not necessary, but when I removed a file, it didn't work correctly until I used `mvn clean` before building.

According to this link: https://stackoverflow.com/questions/30672206/maven-can-not-find-my-custom-maven-archetype
It says that on the Mac, `mvn install` does the same thing as `mvn install archetype:update-local-catalog`, but on Windows, an additional step is needed: `mvn archetype:crawl`.

## Invoking

To create a new project from this archive, use this command from the command prompt:

    mvn archetype:generate \
    -DarchetypeGroupId=com.neptunedreams.archetypes \
    -DarchetypeArtifactId=mac-app-archetype \
    -DarchetypeVersion=1.0-SNAPSHOT \
    -DgroupId=<myGroupId> \
    -DartifactId=<myArtifactId> \
    -Dversion=<myVersion>

Supply your own values for `<myGroupId>`, `<myArtifactId>`, and `<myVersion>`.

You may also do this from the IntelliJ IDE, but the "Add ArchtypeÉ" button doesn't work. Instead, check the "Use Archetype" checkbox without selecting an archetype. The last step will pop up an editable table which lets you set the same values as the command above.

## Notes
1. This archetype was created using the instructions at https://maven.apache.org/guides/mini/guide-creating-archetypes.html
### Build Bug
When attempting to build a project, based on this archetype, using a version of Java later than 9, you get the following error: 

    [ERROR] Failed to execute goal sh.tak.appbundler:appbundle-maven-plugin:1.2.0:bundle (default) on project 
    BeatCounter: Execution default of goal sh.tak.appbundler:appbundle-maven-plugin:1.2.0:bundle failed:
    An API incompatibility was encountered while executing sh.tak.appbundler:appbundle-maven-plugin:
    1.2.0:bundle: java.lang.ExceptionInInitializerError: null
    ...
    [ERROR] : multiple points

Building using the `-e` option: `mvn -e clean install` and you see this in the stack trace:

    Caused by: java.lang.NumberFormatException: multiple points
      at jdk.internal.math.FloatingDecimal.readJavaFormatString (FloatingDecimal.java:1914)
      at jdk.internal.math.FloatingDecimal.parseFloat (FloatingDecimal.java:122)
      at java.lang.Float.parseFloat (Float.java:461)
      at org.apache.commons.lang.SystemUtils.getJavaVersionAsFloat (SystemUtils.java:1117)
      at org.apache.commons.lang.SystemUtils.<clinit> (SystemUtils.java:817)
      at sh.tak.appbundler.CreateApplicationBundleMojo.execute (CreateApplicationBundleMojo.java:390)

It looks like it's trying to get the java version as a floating point number, and it looks like this call is made by the bundler code, so this may be easy to fix. It may be as simple as upgrading to a more recent version of `org.apache.commons.lang.SystemUtils`.

## To Do
Change the bundler from the out-of-date one that I'm currently using to the one at https://github.com/perdian/macosappbundler-maven-plugin. The new one is based on the one I'm using, but is more current.

 
