# Disable StripScript in a preview 
1. Clone the project https://github.com/microfocus-idol/haven-search-components at for example C:\Github\haven-search-components
```
git clone https://github.com/hpe-idol/haven-search-components.git
```

2) Find the StripScript in the code at C:\Github\haven-search-components
```	
  findstr /s /c:"StripScript" *
```

## File
	idol\src\main\java\com\hp\autonomy\searchcomponents\idol\view\IdolViewServerService.java

## Line

```  
	viewParameters.add(ViewParams.StripScript.name(), true);
```

3) Open a cmd at C:\Github\haven-search-components and run 
```
mvn clean package
```

4) Change the pom.xml file at C:\Github\find\pom.xml
```
    <properties>
        <application.buildNumber>HEAD</application.buildNumber>
        <haven.search.components.version>0.12.6</haven.search.components.version>
        <static-resources-dir>${project.build.outputDirectory}</static-resources-dir>
        <npm.binExtension/>
    </properties>
```
5) Recompile Find 
```
mvn clean package 
```

6) Test the WAR at C:\Github\find\webapp\idol\target
