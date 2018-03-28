 Maven archetype for BD components
=================

This document describes how to generate a new TCOMP processing component easily by using the maven archetype.


#### Prerequisites:

1. Java
2. Maven
3. Java integrated development environment like Eclipse or IntelliJ
4. Component project

------

#### Step 1. Installing the maven archetype.

In order to do it, you need to follow those steps :

1. Go into the archetype folder `components/examples/components-api-archetypes/bd-component-archetype`

2. Use the command `mvn clean install` to install the archetype in local


#### Step 2. Creating your first component with the archetype

Follow the instructions in the archetype to fill in the values for the parameters. Suggested values are provided. You should see something like this:

1. Go into your folder `components/components`
2. Use the command :

Then you can use the command :
```
mvn archetype:generate 
-DarchetypeGroupId=org.talend.components 
-DarchetypeArtifactId=tcomp-bd-archetype 
-DarchetypeVersion=<TCompVersion> 
-DarchetypeCatalog=local 
-DgroupId=org.talend.components 
-DartifactId=<NameOfTheComponentFamily> 
-Dversion=0.0.1-SNAPSHOT
-Dpackage=org.talend.components.<NameOfTheComponentFamilyLowerCase>
```
It will generate your archetype into your current folder.

Example:

Run:
```
mvn archetype:generate \
-DarchetypeGroupId=org.talend.components \
-DarchetypeArtifactId=tcomp-bd-archetype \
-DarchetypeVersion=0.21.0-SNAPSHOT \
-DarchetypeCatalog=local \
-DgroupId=org.talend.components \
-DartifactId=Elasticsearch \
-Dpackage=org.talend.components.elasticsearch
```

```
Define value for property 'componentName':  ElasticSearch: : 
Define value for property 'componentNameClass':  ElasticSearch: : 
Dec 12, 2016 6:13:54 PM org.apache.velocity.runtime.log.JdkLogChute log
INFO: FileResourceLoader : adding path '.'
Define value for property 'componentNameLowerCase':  elasticsearch: : 
[INFO] Using property: packageDaikon = org.talend.daikon
[INFO] Using property: packageTalend = org.talend.components
```



#### Step 3: Check classes and start to update them

Your component is now created, you will find the different classes (definition and runtime) under the folder *componentName-definition*, *componentName-integration* and *componentName-runtime*. You will also find the tests for your new component.

You can now update those classes to fit with your own use case.

#### Properties

Classes `input` and `output` properties need to extends `FixedConnectorsComponentProperties`.
Classes `dataset` properties needs to extends `DatasetProperties`.
Classes `datastore` properties needs to extends `DatastoreProperties`.
 

#### Step 4: Implement the runtime part

In the component runtime class, you have 3 choices to implement the runtime.


* "DoFn" => 1 record to another

* "expand" => PTransform : 1 input PCollection => 1 output PCollection.

* "build" => N Input => N Output 
    --> Can call expand
    --> Total control of the flow, we can define exactly what are the inputs and outputs.
    --> need to implement `org.talend.components.adapter.beam.BeamJobBuilder`
    --> need to use `ctx.getPTrasnformName()` to assign PTransform name to pipeline. Can refer to the `FilterRowRuntime` for more information. 
    

#### Step 5: getIconKey()

By default, all the icon key generated by the archetype will be the name of the component in lower case.
You can find the icon name over here : http://talend.surge.sh/icons/

If the name of the component is different from its lower case name, you just need to replace it in the return statement.

Used in all definition classes.