gradle-utils
============

Some utility gradle scripts

localRepo.gradle
----------------

This is a utilily gradle script to publish a single jar into a "repository" directory inside the project root.

Usage: 

1. Copy the `localRepo.gradle` in the root project directory. (I prefer to do this because its usage is a 'one-shot' only, so there is no need to include the script in the build, neither to version control it).

2. Run `gradle -b localRepo.gradle -Dfile=<file>`, where `<file>` may be a path on the local file system, or a http url.
If the file name has a version number, this is all that is needed: at the end of the execution you will find in the "repository" directory a maven-like structure where the group and artifact are the filename.
If a version number cannot be inferred, you must add it as a sytem property on the command line:
	`gradle -b localRepo.gradle -Dfile=<file> -Dversion=<version>`

If you would like a different group name than the file name, add the `-Dgroup=<group>` system property.

After the execution, you can add a local repository in your standard build.gradle:

```gradle
repositories{
	maven{
		url{
			"$rootDir/repository"
		}
	}
}
```

and use the dependency the usual way.

version.gradle
--------------

A simple version strategy to calculate project semantic version number with [gradle-git](https://github.com/ajoberstar/gradle-git).

If current commit is tagged and working directory is clean, the version is the tag.
Otherwise the version is calculated as follows:

* The normal part is the next patch version according to the last git tag. 
* The pre-release is the branch name plus the distance from the last tag.
* The metadata is the abbreviated commit id, eventually followed by ".uncommitted" if the working dir is not clean.

