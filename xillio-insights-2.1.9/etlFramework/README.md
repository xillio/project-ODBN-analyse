# etl-framework
The etl-framework is designed to help structure Xill projects, make them more readable and create resusable code.

The framework uses the principle of pipelines by utilizing the Concurrency plugin to create plugins for Xill projects. It also comes with a Settings robot that uses a JSON file to store the projects settings.
By using a JSON file, more complex configurations are possible.

The idea of using pipelines allows:

- your project to run faster because it runs concurrent.
- ship default mappings with connectors, that can be extended in without changing the defaults.
- changing the flow of your project without changing any code, but just by adding or removing steps in your pipelines.
- greater maintainablity and it creates better readiblity, because it allows a more flat project structure.

## How to use the framework

Utilizing the framework is done by writing one ore more pipeline(s) that call the plugins and additional project functions.
A project contains the etl-framwork folder, a project folder that contains project specific ETL functions (and maybe some helper robots) and one or more pipeline robots.
Please have look at the xillio-insights project for an example.

## How to create a plugin for the etl-framework

### Design rules

* Each plugin should work independed of other plugins.
* A plugin can be a connector but it may also provide other functionality.
* A plugin can contain functions for source, transform or target actions (ETL functions). This functions are always robots that can be used in concurrency pipelines.
* Source functions submit objects to the concurrency queue and are the start of a pipeline.
* Transform functions take objects from the concurrency queue and submit them to the next queue
* Target functions take objects from the concurrency queue and perform a final action onto a target.
* Functions can contain an init function that provide defaults for the config of that function. This is highly recommended because it makes it easier to get an function up and running.
* Besides the ETL functions a plugin may contain helper functions. The helper functions support ETL functions and may also contain functions required for pre- or postprocessing (creating databases, setting indexes etc.).
* The helper functions don't contain do fail blocks, then handling of errors should be done in the ETL functions or the robots that use the helper functions.
* Error handling/logging is done by using pushing Xillio log objects to the queue. Currently the console plugin has a log function supporting these type of objects.
* If you need a more fine grained way of handling your errors, you may implement your own error handling in a project specific error handler.
* If a plugin is a source connector, as a minimum it should contain an export robot (source function) that does all the "crawling" work, formats the objects to the UDM and pushes the objects to the concurrency queue. Please refer to the templates for more details.


### Plugin template

The etl-framework contains a plugin template to help you get started.
* It contains a helpers folder that should contain the helper functions. A robot must be created for each specific task. Picture the Mongo plugin: it has a robots for insert, insertOne and find etc. They are each in a seperate robot.
* It contains a helper robot that should included all the robots in the helpers folder.
* It contains an template for structuring an ETL function, the template contains the explanation. 
* A plugin contains at least one of the following folders: source, transform or target. They contain the actual ETL functions.
