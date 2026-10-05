<!-- source: https://wiki.gentoo.org/wiki/Kubo | group: Gentoo Wiki (Main) | wiki-title: Kubo -->
---
title: Kubo
url: https://wiki.gentoo.org/wiki/Kubo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-15"
fingerprint: "140d156f3c11db8d"
license: CC BY-SA 4.0
---

# Kubo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**IPFS** (Interplanetary File System) is a protocol for peer-to-peer file-sharing. Kubo (formerly go-ipfs) is one of the reference implementations<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> and is available as Gentoo package.

## Installation

### Emerge

`root #``emerge --ask net-p2p/kubo`
### Additional software

Major browsers like [Chrome](https://wiki.gentoo.org/wiki/Chrome), [Chromium](https://wiki.gentoo.org/wiki/Chromium) and [Firefox](https://wiki.gentoo.org/wiki/Firefox) support IPFS through the [IPFS companion](https://docs.ipfs.io/install/ipfs-companion/) extension.

## Configuration

### Initialization

Before the first run, the ipfs repository must be initialized:

`root #``su -s /bin/sh -c "ipfs init -e" ipfs`
### User

When using the default [service](https://wiki.gentoo.org#service) files, the IPFS daemon is started as the ipfs system user. If a regular user wants to interact with the system daemon, the ipfs CLI must be configured to call its API, instead of trying to run in offline mode for the current user:

**`~/.ipfs/api`**

### Profiles

Preset configuration [profiles](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md#profiles) can be applied while the daemon is running, or during [initialization](https://wiki.gentoo.org/wiki/Kubo#Service). For example when a daemon needs to be optimized for a server environment during initialization:

`root #``su -s /bin/sh -c "ipfs init -e --profile server" ipfs`
Similar on a running daemon:

`user $``ipfs config profile apply server`
### Files

- $IPFS\_PATH/config - Dynamic location.
- /var/lib/ipfs/.ipfs/config - System daemon configuration file.
- \~/.ipfs/config - User daemon configuration file.

The config file is JSON formatted and is read during start-up of the daemon or in offline mode. It is not read when ipfs is invoked in CLI mode. The configuration can be edited online with:

`user $``ipfs config edit`
### Service

#### openrc

Add the ipfs daemon to the default runlevel so it'll start on system boot:

`root #``rc-update add ipfs default`
Start it now:

`root #``rc-service ipfs start`
#### systemd

Start and enable the [systemd](https://wiki.gentoo.org/wiki/Systemd) service:

`root #``systemctl start ipfs.service``root #``systemctl enable ipfs.service`
Check the log output for warnings or errors:

`root #``journalctl -u ipfs.service`
## Usage

### Invocation

`user $``ipfs --help````
USAGE
  ipfs  - Global p2p merkle-dag filesystem.
SYNOPSIS
  ipfs [--config=<config> | -c] [--debug | -D] [--help] [-h] [--api=<api>] [--offline] [--cid-base=<base>] [--upgrade-cidv0-in-output] [--encoding=<encoding> | --enc] [--timeout=<timeout>] <command> ...
OPTIONS
  -c, --config               string - Path to the configuration file to use.
  -D, --debug                bool   - Operate in debug mode.
  --help                     bool   - Show the full command help text.
  -h                         bool   - Show a short version of the command help text.
  -L, --local                bool   - Run the command locally, instead of using the daemon. DEPRECATED: use --offline.
  --offline                  bool   - Run the command offline.
  --api                      string - Use a specific API instance (defaults to /ip4/127.0.0.1/tcp/5001).
  --cid-base                 string - Multibase encoding used for version 1 CIDs in output.
  --upgrade-cidv0-in-output  bool   - Upgrade version 0 to version 1 CIDs in output.
  --enc, --encoding          string - The encoding type the output should be encoded with (json, xml, or text). Default: text.
  --stream-channels          bool   - Stream channel output.
  --timeout                  string - Set a global timeout on the command.
SUBCOMMANDS
  BASIC COMMANDS
    init          Initialize local IPFS configuration
    add <path>    Add a file to IPFS
    cat <ref>     Show IPFS object data
    get <ref>     Download IPFS objects
    ls <ref>      List links from an object
    refs <ref>    List hashes of links from an object
  
  DATA STRUCTURE COMMANDS
    dag           Interact with IPLD DAG nodes
    files         Interact with files as if they were a unix filesystem
    block         Interact with raw blocks in the datastore
  
  TEXT ENCODING COMMANDS
    cid           Convert and discover properties of CIDs
    multibase     Encode and decode data with Multibase format
  
  ADVANCED COMMANDS
    daemon        Start a long-running daemon process
    mount         Mount an IPFS read-only mount point
    resolve       Resolve any type of name
    name          Publish and resolve IPNS names
    key           Create and list IPNS name keypairs
    dns           Resolve DNS links
    pin           Pin objects to local storage
    repo          Manipulate the IPFS repository
    stats         Various operational stats
    p2p           Libp2p stream mounting
    filestore     Manage the filestore (experimental)
  
  NETWORK COMMANDS
    id            Show info about IPFS peers
    bootstrap     Add or remove bootstrap peers
    swarm         Manage connections to the p2p network
    dht           Query the DHT for values or peers
    ping          Measure the latency of a connection
    diag          Print diagnostics
    bitswap       Inspect bitswap state
    pubsub        Send and receive messages via pubsub
  
  TOOL COMMANDS
    config        Manage configuration
    version       Show IPFS version information
    update        Download and apply go-ipfs updates
    commands      List all available commands
    log           Manage and show logs of running daemon
  
  Use 'ipfs <command> --help' to learn more about each command.
  
  ipfs uses a repository in the local file system. By default, the repo is
  located at ~/.ipfs. To change the repo location, set the $IPFS_PATH
  environment variable:
  
    export IPFS_PATH=/path/to/ipfsrepo
  
  EXIT STATUS
  
  The CLI will exit with one of the following values:
  
  0     Successful execution.
  1     Failed executions.
  For more information about each command, use:
  'ipfs <subcmd> --help'
```
## Troubleshooting

### receive buffer size

When the configured system maximum buffer size is too small, the following log message will be present during daemon start:

`root #``journalctl -u ipfs.service`
... failed to sufficiently increase receive buffer size (was: 208 kiB, wanted: 2048 kiB, got: 416 kiB). See https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size for details.

The maximum buffer size can be increased by<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>:

`root #``sysctl -w 'net.core.rmem_max=2500000'`
net.core.rmem\_max = 2500000

`root #``echo "net.core.rmem_max=2500000" > /etc/sysctl.d/40-ipfs.conf`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-p2p/kubo`
### Cleanup

Delete the ipfs datastore, configuration and log files:

`root #``rm -rv /var/lib/ipfs/.ipfs`
## External resources

- [concepts](https://docs.ipfs.io/concepts/) - learn more about the IPFS protocol.
- [working with go](https://docs.ipfs.io/reference/go/api/#working-with-go) – list of go-ipfs resources.
- [IPFS companion](https://docs.ipfs.io/install/ipfs-companion/) - browser extensions.
