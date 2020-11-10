# FAST CDA: FHIR tool stack for CDA - Tutorial 
[18.11.2020 - FHIR DevDays Europe](https://www.devdays.com/november-2020/program-november-2020/)

After completing this tutorial, you will be able to:
- Validate a CDA document 
- Convert a CDA document to the Logical Model JSON representation
- Apply FHIRPath expressions to a CDA Document
- Transform CDA to FHIR with the FHIR Mapping Language

Requirements
1. Set up Visual Studio Code
2. Install REST Client Extension for Visual Studio Code
3. Install FHIR tools Extension for Visual Studio Code
4. Java Runtime for running HL7 Java Validator and IG Publisher 
5. Visual Studio Code: Open Command Palette (Windows:Ctrl+Shift+P, OSX: Command+Shit+P): FHIR update Java IG validator and publiser
5. Open Terminal / Command Prompt and change into that folder

## Validate a CDA document

Download [2-7-MedicationCard.xml](https://raw.githubusercontent.com/hl7ch/hl7ch-cda/master/projects/eHealthSuisse/eMedikation/v1.0/2-7-MedicationCard.xml) and open it with Visual Studio Code.

Right mouse click in file and add choose FHIR validate resource (with params) and add the following parameters:

-version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz 

after adding enter the Terminal should show the following output:

```
java -jar validator_cli.jar 2-7-MedicationCard.xml -version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz
FHIR Validation tool Version 5.1.20 (Git# 05056e643e5b). Built 2020-11-05T05:41:42.979Z (5 days old)
  Java:   14.0.2 from /Library/Java/JavaVirtualMachines/jdk-14.0.2.jdk/Contents/Home on x86_64 (64bit). 4096MB available
  Paths:  Current = /Users/oliveregger/Documents/github/cda-core-2.0, Package Cache = /Users/oliveregger/.fhir/packages
  Params: /Users/oliveregger/Downloads/2-7-MedicationCard.xml -version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz
Loading
  Load FHIR v4.0 from hl7.fhir.r4.core#4.0.1 - 4575 resources (00:03.0863)
  Terminology server http://tx.fhir.org - Version 1.0.362 (00:01.0298)
  Load https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz - 113 resources (00:04.0633)
  Get set...  go (00:00.0003)
Validating
  Validate /Users/oliveregger/Downloads/2-7-MedicationCard.xml 00:00.0434
Done. Times: Loading: 00:10.0028, validation: 00:00.0435

Success: 0 errors, 0 warnings, 1 notes
  Information @ ?? : All OK
```
 
## Convert a CDA document to the Logical Model JSON representation

The validator has a parameter to convert XML to JSON. This works also for CDA if the Logical Model package is provided. For this exercise convert the cda-original.xml to JSON.

Right mouse click in file and add choose FHIR validate resource (with params) and add the following parameters:

-version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz -convert -output cda.json

after adding enter the Terminal should show the following output:

```
java -jar validator_cli.jar 2-7-MedicationCard.xml -version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz -convert -output cda.json
FHIR Validation tool Version 5.1.20 (Git# 05056e643e5b). Built 2020-11-05T05:41:42.979Z (5 days old)
  Java:   14.0.2 from /Library/Java/JavaVirtualMachines/jdk-14.0.2.jdk/Contents/Home on x86_64 (64bit). 4096MB available
  Paths:  Current = /Users/oliveregger/Documents/github/cda-core-2.0, Package Cache = /Users/oliveregger/.fhir/packages
  Params: /Users/oliveregger/Downloads/2-7-MedicationCard.xml -version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz -convert -output cda.json
Loading
  Load FHIR v4.0 from hl7.fhir.r4.core#4.0.1 - 4575 resources (00:03.0944)
  Terminology server http://tx.fhir.org - Version 1.0.362 (00:01.0322)
  Load https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz - 113 resources (00:04.0212)
  Get set...  go (00:00.0002)
 ...convert
Done. Times: Loading: 00:09.0728
```

If successful you have the output in cda.json.

## Apply FHIRPath expressions to a CDA Document

The validator has a parameter to apply FHIRPath expressions to CDA document. For this exercise extract the patients given name from the cda.

Right mouse click in file and add choose FHIR validate resource (with params) and add the following parameters:

-version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz -fhirpath recordTarget.patientRole.patient.name.given.dataString

after adding enter the Terminal should show the following output:

```
java -jar validator_cli.jar 2-7-MedicationCard.xml -version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz -fhirpath recordTarget.patientRole.patient.name.given.dataString
FHIR Validation tool Version 5.1.20 (Git# 05056e643e5b). Built 2020-11-05T05:41:42.979Z (5 days old)
  Java:   14.0.2 from /Library/Java/JavaVirtualMachines/jdk-14.0.2.jdk/Contents/Home on x86_64 (64bit). 4096MB available
  Paths:  Current = /Users/oliveregger/Documents/github/cda-core-2.0, Package Cache = /Users/oliveregger/.fhir/packages
  Params: /Users/oliveregger/Downloads/2-7-MedicationCard.xml -version 4.0.1 -ig https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz -fhirpath recordTarget.patientRole.patient.name.given.dataString
Loading
  Load FHIR v4.0 from hl7.fhir.r4.core#4.0.1 - 4575 resources (00:04.0323)
  Terminology server http://tx.fhir.org - Version 1.0.362 (00:01.0323)
  Load https://github.com/ahdis/cda-core-2.0/releases/download/v0.0.3-dev/package.tgz - 113 resources (00:04.0075)
  Get set...  go (00:00.0008)
 ...evaluating recordTarget.patientRole.patient.name.given.dataString
Monika
Done. Times: Loading: 00:09.0967
```
This should return Monika.

If you prefer an interactive way of doing it, upload the cda.json from the previous exercise to niquola fhirpath demo: https://niquola.github.io/fhirpath-demo/#/ or right click in Vistal Studio Code on cda.json and select FHIR open fhirpath.


## Transform CDA to FHIR with the FHIR Mapping Language

The validator has a parameter to perform FHIR Mapping Language transforms. We use the maps defined for swiss e-medication. For this exercise transform the cda to a FHIR document with the map .

Right mouse click in file and add choose FHIR validate resource (with params) and add the following parameters:

-version 4.0.1 -ig http://build.fhir.org/ig/hl7ch/cda-fhir-maps/package.tgz -transform http://fhir.ch/ig/cda-fhir-maps/StructureMap/CdaChEmedMedicationCardDocumentToBundle -output bundle.xml

after adding enter the Terminal should show the following output:

```
java -jar validator_cli.jar2-7-MedicationCard.xml -version 4.0.1 -ig http://build.fhir.org/ig/hl7ch/cda-fhir-maps/package.tgz -transform http://fhir.ch/ig/cda-fhir-maps/StructureMap/CdaChEmedMedicationCardDocumentToBundle -output bundle.xml
FHIR Validation tool Version 5.1.20 (Git# 05056e643e5b). Built 2020-11-05T05:41:42.979Z (5 days old)
  Java:   14.0.2 from /Library/Java/JavaVirtualMachines/jdk-14.0.2.jdk/Contents/Home on x86_64 (64bit). 4096MB available
  Paths:  Current = /Users/oliveregger/Documents/github/cda-core-2.0, Package Cache = /Users/oliveregger/.fhir/packages
  Params: /Users/oliveregger/Downloads/2-7-MedicationCard.xml -version 4.0.1 -ig http://build.fhir.org/ig/hl7ch/cda-fhir-maps/package.tgz -transform http://fhir.ch/ig/cda-fhir-maps/StructureMap/CdaChEmedMedicationCardDocumentToBundle -output bundle.xml
Loading
  Load FHIR v4.0 from hl7.fhir.r4.core#4.0.1 - 4575 resources (00:03.0956)
  Terminology server http://tx.fhir.org - Version 1.0.362 (00:01.0347)
  Load http://build.fhir.org/ig/hl7ch/cda-fhir-maps/package.tgz+  .. load IG from hl7.fhir.cda#dev
+  .. load IG from ch.fhir.ig.ch-emed#current
+  .. load IG from ch.fhir.ig.ch-core#1.0.0
+  .. load IG from ch.fhir.ig.ch-epr-term#current
+  .. load IG from ch.fhir.ig.ch-epr-term#2.0.4
...
 ...success
```

The result should be in bundle.xml
