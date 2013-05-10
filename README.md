# Pixy


## Authors and maintainer

Jenad Jovanonic was the original author of Pixy. The current maintainer is Oliver Klee, pixy@oliverklee.de.


## Available Ant targets

* build: builds the whole project
* clean: cleans up
* test: runs the JUnit unit tests (without generating code coverage data)
* instrument: adds code coverage markers to the generated byte code for the code coverage
* test-coverage: runs the JUnit unit tests and generates code coverage data (needs instrument first)
* coverage-report: creates code coverage reports in HTML and XML in the reports/ directory.


The ant targets are marked with the corresponding dependencies, e.g., if you execute the "coverage-report" target,
the "test-coverage", "instrument" and "build" targets will automatically be executed first.


## Developing Pixy in Eclipse

The most recent version of Eclipse can be obtained from <http://www.eclipse.org/>.


### Importing, adjusting & initial build

1. create a new Java project with the default settings (name: Pixy)
  (--> the "eclipsed" project will reside inside the Eclipse workspace
  after you've finished the following steps; if you want to have it somewhere
  else, export it, delete it, and import it again; see below)

2. disable "build automatically" (from the project menu)

3. from the newly created project's context menu (package explorer), select "Import"
  - File System
    - choose the Pixy home folder (i.e. double click it, then press OK)
    - select it on the following screen (to import everything inside)
      ("create selected folders only"), finish

4. if a red X appears next to the icon of the Pixy project, you forgot to disable
  "build automatically"

5. adjust project properties (from its context menu again):
  - Builders:
    - deselect "Java Builder"
  - Java Build Path:
    - Source Tab:
      - remove existing folder (Pixy)
      - add new folders:
        - Pixy/src/project
        - Pixy/src/java_cup (UPDATE: not necessary any more)
        - Pixy/src/JFlex (UPDATE: not necessary any more)
        - [after the first successful build:]
          Pixy/build/java
      - set default output folder to Pixy/build/class
    - Libraries Tab:
      - Add Class Folder:
        - Pixy/lib
      - Add Jar:
        - Pixy/lib/junit.jar

6. context menu of build.xml: Run / 2.Ant Build...
  - name it "Pixy build"
  - main tab:
    - Base directory = Pixy folder
  - refresh tab:
    - select "refresh upon completion", "project containing the selected resource"
  - targets tab:
    - select "build" target instead of preselected default target
  - run! (should be successful)


### Building and cleaning the project with Ant from within Eclipse

1. Project > Properties > Builders
2. Deactivate the Java Builder.
3. New ...
4. Select "Ant builder"
3. Name it "Ant build" or "Pixy build" (or any other suitable name).
5. In the Main tab, select the build.xml in the project directory as Buildfile and the project directory as Base directory.
6. In the Targets tab for "Manual build", select "build".
7. In the Targets tab for "During a clean", select "clean".
8. OK the changes for both dialogs.

You then can build the project using Project > Build Project and Clean the project using Project > Clean ... (You might need to refresh to project first.)

Or you can build the project using the command line from within the project main directory:

    ant build


### Running Pixy from within Eclipse

To run the application, you first need to build it. After that is done, create a suitable run configuration:

1. Run > Run Configurations...
2. Right-click on "Java Application" > New (or double-click on "Java Application")
3. Enter a name, e.g., "Run Pixy"
4. The main project should be your Pixy project.
5. As Main class, press "Search..." and select the Checker class.
6. In the Arguments tab, set the following as Program arguments: ${project_loc}/getstarted.php
7. Still in the Arguments tab, set the following VM arguments: -Dpixy.home=${project_loc}
8. Click on "Run".

After the configuration is complete, you can run Pixy by just using Run > Run (CTRL + F11).


### Running the unit tests from within Eclipse

This also requires that you have build the application.

1. Run > Run Configurations...
2. Right-click on "JUnit" > New (or double-click on "JUnit")
3. Enter a name, e.g., "Pixy unit tests"
4. In the Test tab, select the "Run all tests ..." radio button and select the "test" folder in your Pixy project.
5. In the Arguments tab, set the following VM arguments: -Dpixy.home=${project_loc}
6. Click on "Run".

After the configuration is complete, you can run Pixy by right-clicking the "test" folder and selecting Run As > 3 JUnit test

Note: Currently, this is broken as one test case completely fails (although it runs when using the "test" ant target
from the command line).


### Development building

1. context menu of build.xml: Run / External tools
  - duplicate existing build configuration
  - rename the copy to "Pixy javac"
  - replace target "build" with "javac"
  - run


### Exporting

1. don't forget to clean up first
2. as zipfile or to file system
3. check if it contains a .project file (that's how it should be, otherwise
  you can't import it back to Eclipse easily)

### Importing again
1. unzip file outside eclipse (if zipped); that location is not just temporary,
  but will be used by eclipse as project home
2. don't create a project
3. context menu of package explorer: Import... Existing project into workspace,
  select unzipped home folder, finish

## Usage (Eclipse)

Thanks to Chuck Burgess (cburgess at PROGRESSRAIL dot com) for
contributing these instructions.

To set up Pixy as an "External Tool" to use inside Eclipse (tested on
v3.1.2), perform these steps:

1. From the "Run" menu, choose "External Tools", then "External Tools"
  again.  This brings up the dialogue where you can edit/create
  "External Tools" items.
2. On the left-side menu, highlight the "Programs" heading and click
  the "New" button.
3. Populate the "Name" field on the main top-center area with whatever
  name you want to use to recognize this program for yourself (i.e.
  "Pixy Vulnerability Scanner for PHP").
4. On the "Main" tab, I populated these items in this manner:
     - Location: 			${workspace_loc}/Pixy/run.bat
     - Working Directory:  	${workspace_loc}${project_path}
     - Arguments:  		"${workspace_loc}${resource_path}"
  Notice the quotes in the "Arguments" value... they are needed here,
  though not allowed in the other values.  If you physically put the
  Pixy code in your Eclipse workspace, you can use the ${workspace_loc}
  variable in the path to the "run" or "run.bat" file in the "Location"
  item, rather than having to hardcode the path.  Otherwise, you should
  hardcode the path in the "Location" item, but not in the "Working
  Directory" or "Arguments" items...  (these items refer to your workspace
  area/items specifically).
5. No changes should be necessary on the "Refresh", "Environment", or
  "Common" tabs.

To run Pixy on a workspace PHP file, just highlight the file (either on
the left-side Navigator menu listing, or else the opened file's tab in
the center view area).  Click "Run -> External Tools", and choose your
Pixy item's name.  The output from its run will appear in the "Console"
view at the bottom of the screen layout.  Remember, Pixy runs against
the file you have HIGHLIGHTED, which is not necessarily the one you
have OPEN.


## Pixy and PhpParser

Pixy makes use of the sub project PhpParser. It already brings the compiled classes from that project. PhpParser resides [at Github](https://github.com/oliverklee/phpparser "PhpParser at Github").


## Modifications to JFlex 1.4.1
- marked with "NJ"
- modified files:
  - skeleton.default
  - Emitter.java
- reasons:
  - emulation of Flex's yymore()
  - in case of error: also include file name and line number in the output message
- state stacks are emulated without modifying the JFlex sources, simply by
  including the relevant data structures into the specification file

## Modifications to Cup 10k
- marked with "NJ"
- modified files:
  - emit.java
- reasons:
  - indices for terminals and nonterminals in the generated symbols class shall not overlap
  - nonterminals in the generated symbols class shall be public
  - rule actions shall have access to
    - the production's symbol index (i.e. the index of the symbol on the left side)
    - the production's symbol name
    - the production's length
   since that makes the generation of explicit parse trees much easier

## Licenses
* Pixy: GPL V3
* jFlex: GPL V2
* Cup: custom license, derived from "Standard ML of New Jersey", GPL-compatible
* CLI: Apache license
* Cobertura: GPL V2
* Log4J: Apache license
* Jakarta: Apache license
* ASM, ASM-Tree: BSD
* rest: GPL V2