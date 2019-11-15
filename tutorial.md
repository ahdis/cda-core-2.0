# FAST CDA: FHIR tool stack for CDA - Tutorial 
[22.11.2019 - FHIR DevDays Europe](https://www.devdays.com/amsterdam/schedule-2019/#event-810)

After completing this tutorial, you will be able to:
- Validate a CDA document 
- Convert a CDA document to the Logical Model JSON representation
- Apply FHIRPath expressions to a CDA Document
- Transform CDA to FHIR with the FHIR Mapping Language

Requirements
1. Java Runtime: JRE 8 (or greater) installed on computer
2. Download Content of Release https://github.com/ahdis/cda-core-2.0/releases/tag/v0.0.2-dev in a folder on your  compjuter.
3. Open Terminal / Command Prompt and change into that folder

## Validate a CDA document

See requirements first to start the tutorial. The cda-core-2.0 release contains CDA examples. For this exercise validate the cda-ch.xml with the org.hl7.fhir.validation.cli.jar from here, it should have 0 errors.

```
java -jar org.hl7.fhir.validation.cli.jar -version 4.0.1 -ig package.tgz cda-ch.xml 
FHIR Validation tool Version 4.0.32-SNAPSHOT
.. connect to tx server @ http://tx.fhir.org
  .. definitions from hl7.fhir.r4.core#4.0.1
+  .. load IG from package.tgz
  .. validate [resources/examples/cda-ch.xml]
Success...validating resources/examples/cda-ch.xml:  error:0 warn:0 info:20
```
 
## Convert a CDA document to the Logical Model JSON representation

The validator has a parameter to convert XML to JSON. This works also for CDA if the Logical Model package is provided. For this exercise convert the cda-original.xml to JSON.

```
java -jar org.hl7.fhir.validation.cli.jar -version 4.0.1 -ig package.tgz -convert -output cda-original.json cda-original.xml
```

If successful you have the output in cda-original.json.

## Apply FHIRPath expressions to a CDA Document

The validator has a parameter to apply FHIRPath expressions to CDA document. For this exercise extract the patients given name from cda-original.xml. If you prefer an interactive way of doing it, upload the cda-original.json from the previous exercise to niquola fhirpath demo: https://niquola.github.io/fhirpath-demo/#/ 

```
java -jar org.hl7.fhir.validation.cli.jar -version 4.0.1 -ig package.tgz -fhirpath recordTarget.patientRole.patient.name.given.dataString resources/examples/cda-original.xml
```

This should return Henry.

## Transform CDA to FHIR with the FHIR Mapping Language

The validator has a parameter to perform FHIR Mapping Language transforms. The maps are provided in the maps directory. For this exercise transform ccda.xml to a FHIR document with the map ClinicalDocument to ccda-fhir.xml.

```
java -jar org.hl7.fhir.validation.cli.jar -version 4.0.1 -ig package.tgz -transform http://hl7.org/fhir/cda/mapping/ClinicalDocumentToFHIR -ig maps -log test.txt -output ccda-fhir.xml ccda.xml 
Start Transform http://hl7.org/fhir/cda/mapping/ClinicalDocumentToFHIR
Group : ClinicalDocumentBundle; vars = source variables [source: (ClinicalDocument)], target variables [target: (Bundle)], shared variables []
...
 ...success
```

The result should be in ccda-fhir.xml
