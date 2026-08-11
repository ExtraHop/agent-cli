# ExtraHop CLI

`excli` invokes the ExtraHop tools from a shell with JSON input and output. It is intended for context optimization, shell pipelining, and terminal workflows. The tool set is identical to `exmcp`, and [ExtraHop/agent-mcp](https://github.com/ExtraHop/agent-mcp) documentation is generally applicable.

**Requires ExtraHop firmware 26.3 or later. For earlier firmware versions, see the [0.0.111 release](https://github.com/ExtraHop/agent-cli/tree/bf5c541bf3880a41ef1fc5198f913993cf3a2b9a/dist).**

## Usage

```text
$ excli --help
Runs REST API tools via the CLI.

Usage:
  excli -listtools
  excli -jsonschema
  excli -help
  excli TOOL -help
  excli TOOL [-json JSON]

Options:
  -listtools       List tools.
  -jsonschema      Print tool JSON schemas.
  -help            Show help.
  -version         Print the version.

Recommended workflow:
  1. Run excli -listtools
  2. Run excli TOOL -help before using any tool
  3. Use the tool's help text to build the -json input
  4. Redirect output to files; tool responses can be large

Configure exactly one credential family: Rx360 uses EXTRAHOP_CLIENT_ID and
EXTRAHOP_CLIENT_SECRET; RxEnterprise uses EXTRAHOP_API_KEY.

Rx360 Environment:
  EXTRAHOP_CLIENT_ID              OAuth2 client ID for Rx360.
  EXTRAHOP_CLIENT_SECRET          OAuth2 client secret for Rx360.

RxEnterprise Environment:
  EXTRAHOP_API_KEY                API key for RxEnterprise.
  EXTRAHOP_INSECURE               Set true to skip RxEnterprise TLS certificate verification.

Both Environments:
  EXTRAHOP_HOST                   Rx360 tenant or RxEnterprise appliance host.
  EXTRAHOP_PCAP_DOWNLOAD_DIRECTORY Directory for download_pcap output files.

Examples:
  excli search_detections -help
  excli search_detections -json '{"from":-3600000,"limit":10}'
```

## Tools

| Tool | Read/Write | Destructive | Description |
| --- | --- | --- | --- |
| `get_exmcp_version` | Read | No | Get the version of the running ExtraHop MCP server. |
| `create_investigation` | Write | No | Create a new investigation to group related detections together for collaborative analysis. Investigations allow analysts to track and manage security incidents by associating detections, assigning ownership, and recording assessments and notes. Returns the ID of the new investigation. |
| `update_detection` | Write | No | Update a detection. All fields are optional. Resolution is only valid when the detection status is (or is being set to) "closed". Setting status to a non-closed value clears any existing resolution. |
| `get_detectiontypemetadata` | Read | No | Get metadata for a detection type by its type key. Returns the display name, MITRE ATT&CK technique IDs, categories, and typed property definitions. Only active detection formats and active properties are returned. Use to understand what a detection type means and what properties its activity entries will carry. |
| `get_appliance_metadata` | Read | No | Retrieve metadata about the firmware running on the ExtraHop appliance. |
| `get_extrahop_help_docs_url` | Read | No | Get the ExtraHop documentation URL for the connected appliance. |
| `create_tuningrule` | Write | Yes | Create a tuning rule. |
| `get_detection` | Read | No | Retrieve a specific detection. |
| `preview_tuningrule` | Read | No | Retrieve a preview that describes how many detection log entries will be hidden by a tuning rule. |
| `search_detections` | Read | No | Search for detections. |
| `search_devices` | Read | No | Retrieve all active devices that match specific criteria. |
| `search_networkusers` | Read | No | Search for network users. |
| `get_device` | Read | No | Get full details for a device by ID aka OID. |
| `search_devicegroups` | Read | No | Perform a filtered search for collections of devices |
| `search_records` | Read | No | Search transaction records (HTTP, DNS, SSL, etc.). |
| `download_pcap` | Read | No | Download packets from the packet search endpoint to a local file in the configured pcap-download-directory. The filename is generated as a UUID. Streams the response body to that file and returns metadata. The consumer is responsible for cleaning up the pcap file as needed. |
| `search_devicetags` | Read | No | List all device tags on the system. Tags are user-defined labels that can be assigned to devices for grouping and filtering. |
| `list_devicetags_for_device` | Read | No | List tags assigned to a specific device. Tags are user-defined labels for grouping and filtering. |
| `list_devices_in_devicegroup` | Read | No | Retrieve all devices in the device group that were active within a specific time window. Returns a compact summary for each device. To get full details for a device use get_device. |
| `execute_metric_query` | Read | No | Query metrics (time series or total) for ExtraHop objects. |
| `search_metric_catalog` | Read | No | Search the local ExtraHop metric catalog. The catalog lists most builtin metrics that the appliance supports. Custom metrics are not included. |
| `assign_devicetag_to_devices` | Write | No | Assign a tag to one or more devices. The tag must already exist. |
| `unassign_devicetag_from_devices` | Write | No | Unassign a tag from one or more devices. The tag must already exist. |
| `get_eql_syntax` | Read | No | Get the EQL (ExtraHop Query Language) query syntax reference shared by every EQL search tool. |
| `get_eql_schema` | Read | No | Get the EQL search schema for a resource: the fields that can be queried and the operators available. |
| `get_eql_fieldvalues` | Read | No | Get the expected values for one or more EQL fields on a resource. |
| `search_detectionlogs` | Read | No | Search detection log entries with an EQL query and return matching entries. |

## Related Repositories

This repository is a member of ExtraHop's agent repositories:

- ExtraHop/agent-cli (this repository)
- [ExtraHop/agent-mcp](https://github.com/ExtraHop/agent-mcp)
- [ExtraHop/agent-skills](https://github.com/ExtraHop/agent-skills)
