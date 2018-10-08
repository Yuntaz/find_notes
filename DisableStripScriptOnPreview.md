# Disable StripScript in a preview 
1. Clone the project https://github.com/microfocus-idol/haven-search-components at for example C:\Github\haven-search-components
```
git clone https://github.com/hpe-idol/haven-search-components.git -b Develop
```
2. Find out the version you have in local. You will find this on the pom.xml document between the xml brackets of Version. For my example, I will use this version: 0.62.0-SNAPSHOT.

3. Find the StripScript in the code at C:\Github\haven-search-components
```	
  findstr /s /c:"StripScript" *
```
4. Change the file in the project
## File
idol\src\main\java\com\hp\autonomy\searchcomponents\idol\search\HavenSearchAciParameterHandlerImpl.java

## Line
Change the True to False in that file and save it
```  
	viewParameters.add(ViewParams.StripScript.name(), true);
```

5. Open a gitshell cmd at C:\Github\haven-search-components and run 
```
mvn clean package
```

6. Now, Change the pom.xml file at C:\Github\find\pom.xml and use the local version, in my example, will be 0.62.0-SNAPSHOT.
```
    <properties>
        <application.buildNumber>HEAD</application.buildNumber>
        <haven.search.components.version>0.62.0-SNAPSHOT</haven.search.components.version>
        <static-resources-dir>${project.build.outputDirectory}</static-resources-dir>
        <npm.binExtension/>
    </properties>
```
7. Recompile Find 
```
mvn clean package 
```

8. Test the WAR at C:\Github\find\webapp\idol\target
