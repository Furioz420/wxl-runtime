# wxl-runtime

[Build compatibility and release gate](BUILDING.md)

Shared FrameScript and custom-packet services for WarcraftXL v1.1 extensions. The module keeps one
owner for the client packet hook and lets feature modules register handlers through published APIs.

## Services

- `wxl.framescript` v1 registers native Lua functions, scripts, and persistent client CVars.
- `wxl.network` v1 registers and sends WarcraftXL custom opcodes.
- `wxl.network` v1 also adds non-owning listeners without replacing a feature's packet handler; native WoW packets still reach the client.

## Requirements

- WarcraftXL v1.1 with the FrameScript, network, and public-opcode core contracts.
- 32-bit World of Warcraft 3.3.5a client, build 12340.

This module has no client DB2/DBC payload and no server component by itself. Features consuming the
network service still require a server implementation with the same opcodes and packet layout.

## Installation

Install with WXL Hub. The release ZIP contains `wxl-runtime.dll`; the Hub places it under
`Extensions\\wxl-runtime`. Restart the client after installing or updating a native module.

For manual installation, extract the release ZIP to that same directory. FrameScript and network
services are both required and enabled by this version.

## Building

The release workflow builds this source as the `wxl-runtime` extension target in WarcraftXL v1.1.
Until the prerequisite core API change is merged upstream, build it against that review branch.

## Integration and release checks

Build the `wxl-runtime` Win32 Release target against the matching core FrameScript and `wxl.network` v1 interfaces. The proposed 1.1 release workflow packages only `wxl-runtime.dll`; the removed config switches no longer disable either service. No DB2, UI, or server data ships here. Features using custom packets must provide matching server opcode handlers.

Check startup logs for both published services, register a small FrameScript function through a dependent test module, and verify a custom packet round trip with a compatible server consumer. Confirm native observed packets still reach WoW. Keep the prior Runtime and dependent DLLs for rollback as one group. The standalone `main` workflow still builds against moving upstream `v1.1`; pin the merged core API and validate that ZIP before release.

## Credits

The WXL core ABI and original module interfaces come from WarcraftXL contributors. The local v1.1 integration commits in this snapshot are attributed to Furioz in the integration history. Preserve source-file notices and the GPL-3.0-or-later `LICENSE` when redistributing source or binaries.
