# cda-core-2.0

This is the source for the cda-core-2.0 Implementation Guide. The Implementation Guide is then produced with the ig publisher.

## Mapping with cda-core-2.0

In the mapping directory are files from the [ccda-to-fhir](https://github.com/HL7/ccda-to-fhir) project. Current change is, that the section handling is not used from ccda.

To run the maps the [HL7 JAVA FHIR validator](https://confluence.hl7.org/display/FHIR/Using+the+FHIR+Validator#UsingtheFHIRValidator-ValidatingCDADocuments) can be used. 

To run the maps you need:
- updated validator with latest pull request integrated, see https://github.com/ahdis/org.hl7.fhir.core/releases/, copy the jar to this directory and rename it to org.hl7.fhir.validation.cli.jar
- intermediate release of the cda-core-2.0 package, see https://github.com/ahdis/cda-core-2.0/releases/, copy the package.tgz  

for CDA to FHIR the maps can now be run in the following way to transform the sample ccda.xml to a bundle.xml:

```
java -jar org.hl7.fhir.validation.cli.jar ./resources/examples/ccda.xml -transform http://hl7.org/fhir/cda/mapping/ClinicalDocumentToFHIR -version 4.0.1 -ig package.tgz -ig maps -log test.txt -output bundle.xml
```
