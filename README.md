# ExtraHop CLI

`excli` invokes the ExtraHop tools from a shell with JSON input and output. It is intended for context optimization, shell pipelining, and terminal workflows. The tool set is identical to `exmcp`, and [ExtraHop/agent-mcp](https://github.com/ExtraHop/agent-mcp) documentation is generally applicable.

## Help Output

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
| `create_investigation` | Write | No | Create a new investigation to group related detections together for collaborative analysis. Investigations allow analysts to track and manage security incidents by associating detections, assigning ownership, and recording assessments and notes. |
| `search_detections` | Read | No | Search for security detections with filters like category, status, assignee, and type. Returns a compact summary for each detection. Use get_detection and search_detectionactivity for more details. |
| `get_detection` | Read | No | Get details for a detection by ID. Pair with search_detectionactivity for the correlated timeline. See search_detections tool description for documentation on how to pivot from participants to device metadata or records. |
| `update_detection` | Write | No | Update a detection. All fields are optional. Resolution is only valid when the detection status is (or is being set to) "closed". Setting status to a non-closed value clears any existing resolution. |
| `search_detectionactivity` | Read | No | Get the timeline of observed activity that makes up the detection, with correlated participants and properties. Use alongside get_detection when investigating a detection in detail. See search_detections tool description for documentation on how to pivot from participants to device metadata or records. |
| `get_detectiontypemetadata` | Read | No | Get metadata for a detection type by its type key. Returns the display name, MITRE ATT&CK technique IDs, categories, and typed property definitions. Only active detection formats and active properties are returned. Use to understand what a detection type means and what properties its activity entries will carry. |
| `get_appliance_metadata` | Read | No | Retrieve metadata about the firmware running on the ExtraHop appliance. |
| `get_extrahop_help_docs_url` | Read | No | Get the ExtraHop documentation URL for the connected appliance. |
| `search_devices` | Read | No | Search for devices with filters and active time range. Returns a compact summary for each device. To get full details for a device use get_device. |
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

## Related Repositories

This repository is a member of ExtraHop's agent repositories:

- ExtraHop/agent-cli (this repository)
- [ExtraHop/agent-mcp](https://github.com/ExtraHop/agent-mcp)
- [ExtraHop/agent-skills](https://github.com/ExtraHop/agent-skills)
