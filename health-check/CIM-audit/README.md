# CIM Audit

Version history of the CIM Data Model Audit dashboard.

| Version | Panels | Changes |
| --- | --- | --- |
| [v1](CIM_Data_Model_Audit_v1.xml) | 8 | Initial version: four candidate-index panels and four data-model field-audit panels; light theme. |
| [v2](CIM_Data_Model_Audit_v2.xml) | 5 | Candidate panels removed; index inventory added at the top; dark theme; explanatory text removed. |
| [v3](CIM_Data_Model_Audit_v3.xml) | 9 | Latest: five separate Endpoint dataset panels; one-column index inventory moved to the end. |

## Latest version

Use `CIM_Data_Model_Audit_v3.xml`. Endpoint datasets: Processes, Filesystem, Services, Registry and Ports. Web, Network Traffic and DNS have separate panels. Field audits display distinct counts, populated-event counts and percentages by index.

## Import

Create a Classic / Simple XML dashboard in Splunk, open Edit > Source, paste the selected XML and save. Select the time range and click Submit. Run in the app context where the required CIM mappings and knowledge objects are available.

The XML structure has been checked. Runtime validation takes place on the target Splunk deployment. Earlier versions are retained as historical snapshots.
