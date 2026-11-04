# Monitoring CCminer

Quick guide for configuring and monitoring CCminer instances via the built-in API.

## Disclaimer & Support

* This guide and associated scripts are provided as-is with **no official support or warranties**. Use at your own risk.

## Enabling the API on CCminer

By default, CCminer does not enable remote API access and only accepts connections from localhost (`127.0.0.1`). 

To enable API access across your local network, add or modify the following properties in your `config.json` file:

```json
{
  "api-allow": "192.168.1.0/24",
  "api-bind": "0.0.0.0:4068"
}
```

* **`api-allow`**: Specifies the allowed IP range (subnet CIDR). Adjust this to match your local network address range. To restrict access to a single host, use `/32` notation (e.g., `"192.168.1.25/32"`).
* **`api-bind`**: Binds the API service to the specified IP address and port. Using `0.0.0.0` allows connections on all network interfaces.

## Interacting with the CCminer API

You can remotely interact with the CCminer API using the Perl helper script `api.pl`.

### Syntax
```bash
./api.pl --cmd COMMAND --address ADDRESS --port PORT
```

*Note: You can shorten `--cmd`, `--address`, and `--port` to `-c`, `-a`, and `-p` respectively.*

### Available Commands
* `summary`
* `threads`
* `pool`
* `histo`
* `hwinfo`
* `meminfo`
* `scanlog`
* `switchpool`
* `quit`

The `summary` and `pool` commands are the most useful options for monitoring performance. The script returns semicolon-separated values (`;`) suitable for automated parsing.

## Batch Monitoring with `check-all`

For managing multiple miners, use the included `check-all` script to aggregate status data into JSON format:

1. Open `check-all` and specify a space-separated list of target miner IP addresses on line 4.
2. Run the script to generate output and format key data using `jq`.

### Example Command
```bash
./check-all | jq -c '.[] | [.HOST, .POOL, .KHS]'
```

This command outputs a single line per miner displaying:
* Target Host IP
* Active Mining Pool (configured in `config.json`)
* Current Hashrate (in kH/s)

### Advanced Monitoring Dashboard Example
To continuously refresh and colorize your miner status within a Linux terminal or `screen` session:

```bash
watch --color -n 60 "/root/api/check-all | jq -c '.[] | [.HOST,.POOL,.KHS]' | sed '/pool.verus.io/s/.*/\x1B[32m&\x1B[0m/g; /pool.verus.io/!s/.*/\x1B[31m&\x1B[0m/g' | column"
```
*(This setup highlights active connections to `pool.verus.io` in green, while alerting other states in red).*

### Available Data Fields in `check-all`

| Field | Description |
| :--- | :--- |
| `HOST` | IP address of the queried host miner. |
| `POOL` | Active mining pool hostname/URL. |
| `USER` | Wallet address and worker name. |
| `KHS` | Hashrate performance in kH/s. |
| `DIFF` | Mining job difficulty level. |
| `ACC` | Count of accepted shares submitted to the pool. |
| `REJ` | Count of rejected shares. |
| `WAIT` | Pool response wait time in seconds (typically `0` during active job processing). |
| `UPTIME` | Total runtime in seconds since last restart. |
| `TS` | Timestamp in Unix Epoch format. |

---
*Use at your own risk! No warranties or liabilities are provided for these tools.*
