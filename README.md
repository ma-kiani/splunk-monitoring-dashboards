# Splunk Monitoring Dashboards for SOC Operations

Splunk dashboards for Windows, Linux, Microsoft IIS, Cisco switches, and Common Information Model (CIM) data quality. This repository contains Classic / Simple XML dashboards for SOC analysts, detection engineers, and Splunk administrators who need to check log coverage, investigate ingestion gaps, and review the data behind their security monitoring.

## Dashboard catalog

| Area | Dashboard | What it covers |
| --- | --- | --- |
| Windows | [Windows Overview](windows/windows_overview.xml) | Windows and Sysmon host coverage, missing Sysmon logs, Windows event channels, ingestion delay, and sourcetype distribution. |
| Linux | [Linux Overview](linux/linux_overivew.xml) | Linux log coverage, auditd visibility, silent hosts, log source categories, ingestion delay, and audit field extraction. |
| Web servers | [IIS Overview](webserver/iis_overview.xml) | HTTP request volume, active IIS servers, source IPs, response codes, P95 response time, slow or failing servers, and requested URI paths. |
| Network devices | [Cisco Switch Overview](cisco/cisco_overview.xml) | Cisco syslog volume, active and silent switches, observed severity levels, and field extraction quality. |
| SIEM health | [CIM Data Model Audit — v3](health-check/CIM-audit/CIM_Data_Model_Audit_v3.xml) | Field cardinality and population by index across Web, Network Traffic, DNS, and five Endpoint datasets, plus an index inventory. |

The Overview dashboards use a dark theme. Several panel labels and operational notes are in Persian; the SPL queries and field names retain their original form.

## CIM data model audit

The latest CIM audit has nine panels:

- **Web** — URL, URI path, URI query, source and destination, HTTP method, status, user agent, and related fields.
- **Network Traffic** — Source and destination addresses and ports, transport, action, application, traffic bytes, and related fields.
- **Endpoint Processes** — Process names, command lines, paths, identifiers, parent processes, users, and destinations.
- **Endpoint Filesystem** — File names, paths, hashes, sizes, actions, users, and destinations.
- **Endpoint Services** — Service names, paths, start modes, status, users, and destinations.
- **Endpoint Registry** — Registry paths, keys, value names and data, actions, users, and destinations.
- **Endpoint Ports** — Listening ports, connected sources, transport, state, process identifiers, users, and destinations.
- **Network Resolution (DNS)** — Queries, answers, clients, resolvers, query and record types, reply codes, and message types.
- **All Indexes** — A simple index-name inventory.

Each data-model panel groups results by index and shows distinct counts, populated-event counts, and field population percentages. This makes it easier to spot missing CIM mappings, unexpectedly low field cardinality, and differences between log sources before relying on them in a detection.

The dashboard includes a time picker, an index filter, and a choice between a full-range search and acceleration summaries only. Field population counts include non-null placeholders such as `unknown`; inspect the underlying values when reviewing a mapping. The inventory uses Splunk's REST command and depends on the account's access to the configured search peers.

[Earlier versions and change history](health-check/CIM-audit/README.md) are retained alongside v3.

## Install a dashboard

1. Open the XML file in GitHub and select **Raw** to copy its contents.
2. In your target Splunk app, create a **Classic / Simple XML** dashboard.
3. Open **Edit → Source**, replace the source with the copied XML, and save.
4. Select a time range and any available filters, then click **Submit**.

Use the app context where your field extractions, aliases, macros, and data models are shared. Dashboard Studio uses a different format; these XML files are intended for Classic dashboards.

## Data and dependencies

| Dashboard | Expected data or dependency |
| --- | --- |
| Windows Overview | `windows` and `sysmon` indexes, with Windows channel fields available for the channel-coverage panel. |
| Linux Overview | Primarily the `linux` index; the log-source trend also searches `apache`, `nginx`, and `zabbix`. Auditd checks use `sourcetype=linux:audit` or an audit log source path. |
| IIS Overview | The `iis` index, extracted IIS fields, and CIM-normalized `Web.Web` data. Response-time panels use `time_taken`. |
| Cisco Switch Overview | A `cisco_index` search macro whose definition is the Cisco index name, such as `cisco`, plus the Cisco fields referenced in the queries. |
| CIM Data Model Audit | Accessible `Web`, `Network_Traffic`, `Network_Resolution`, and `Endpoint` data models. Accelerated summaries are needed when summaries-only mode is selected. |

Update index names and field mappings to match your deployment. The IIS dashboard contains drilldown links to `web_investigation`; that companion dashboard is not included here. The Cisco dashboard depends on `cisco_index`; its macro definition must be created in the target app.

## Validation and contributions

These dashboards help assess the logs that Splunk can see. Observed hosts are not a complete asset inventory, and an empty panel alone does not establish that a control is missing. Check permissions, time ranges, field extraction, CIM tags, and data-model acceleration when reviewing results.

XML structure has been checked. Validate SPL results and search cost against your own data before adding a dashboard to a SOC monitoring routine.

For an issue or contribution, include the dashboard filename, affected panel, Splunk version, relevant sourcetype, and a sanitized example. Keep project-specific hostnames, addresses, credentials, and raw customer logs out of submissions.
