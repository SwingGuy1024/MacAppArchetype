# Mac Application Maven Archetype

This is a maven archetype to create a bundled stand-alone application for the Macintosh.

## Build

### Before building
1. You may need to go into `Info.plist` and change ${mainClass} to the fully qualified class name of the main class. This can be done by Maven with a script, but I don't know how to do that right now.
2. Replace the empty `appIcon.icns` file with a real Icons file.


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

You may also do this from the IntelliJ IDE, but the "Add Archtype…" button doesn't work. Instead, check the "Use Archetype" checkbox without selecting an archetype. The last step will pop up an editable table which lets you set the same values as the command above.


 