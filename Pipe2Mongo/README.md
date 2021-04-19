# Xillio Pipe2Mongo #

## Summary ##
the **Pipe2Mongo** connector reads data extraction files created by the **Xillio Pipe** file share extraction application and stores them in the Xillio UDM format in a **MONGO Database**

## Pre requisites ##
*.json formatted data files according to the Xillio Pipe data specifications

## Configuration ##

Configuration of the connector is done via the configuration files stored in a configurable folder.

The configuration folder (configPath) location can be set in:
    /config.json 

The configurations are stored in two different files:

    (configPath)/repositories.json

    (configPath)/general.json

## repositories.json ##
Multiple repositories can be defined
 

    { "repositoryName" :{  } } #a unique "repositoryName" of the source /repository

    "type": "pipe"  #must be "pipe for this pipe2Mongo connector
	
	"path" : "E:/Project/myProject/pipe/files", # the path to the location of the pipe datafiles
	
**mongo**

		specify the Mongo database connection credentials, database name and collection name

### general.json ###

If multiple repositories exist, but not all of them need to be (re)processed you can exclude them from the Run.
Set the excludedRepositories element in the configuration file with the repositoryNames to exclude in the Run.
Separate multiple repositoryNames with a , (Comma)

Example:

    excludedRepositories" : ["test", "test2"]
    
## Tips ##
- Use multiple repositories for different file share sources.
- Repositories can be exported to its own database or collection but that is not a must
- recommended database name = "udm_[projectName]" and collection name "documents" (=common naming convention in XIll connectors)

- Makes sure both file as folder data files are in the data folder when running the Connector

- Verify the Pipe data files for error files. Fix the errors if possible
- Do not leave the error files in the "path" folder during the Pipe2Mongo Run
