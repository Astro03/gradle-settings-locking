gradle-settings-locking
=======================

Toy project to illustrate that dependencies loaded into the settings scripthandler can be missing from dependency lockfiles,
or that the lockfile naming/locations are inconsistent.

Note that a dynamically-versioned Gradle Enterprise is loaded in the settings scripthandler.
It will be absent completely from Example One below.

## Example One: Default behavior
Invoke:

`$ ./gradlew resolveAndLockAll --write-locks`

Expected: 
1. Separate files for settings vs. buildscript scripthandler dependencies
2. GE plugin present in settings scripthandler lockfile

Actual:
1. Single file at `gradle/dependency-locks/buildscript-classpath.lockfile` contains buildscript dependencies
2. Settings scripthandler information is missing; only buildscript classpath is written to the lockfile

## Example Two: With `ONE_LOCKFILE_PER_PROJECT` feature preview
Uncomment the last line of `settings.gradle`, then invoke:

`$ ./gradlew resolveAndLockAll --write-locks`

Expected:
1. Separate files for settings vs. buildscript scripthandler dependencies
2. Both files are in the root project folder
3. GE plugin present in settings scripthandler lockfile

Actual:
1. Separate files exist: `buildscript-gradle.lockfile` contains buildscript dependencies; 
`gradle/dependency-locks/buildscript-classpath.lockfile` now contains settings dependencies
2. Use of `gradle/dependnecy-locks` is unexpected with `ONE_LOCKFILE_PER_PROJECT` feature preview
3. GE plugin is present in settings scripthandler lockfile (success!)
