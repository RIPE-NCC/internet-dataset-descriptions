# RIPE RIS MRT Files

RIPE RIS is a project that collects and stores a longitudinal archive of Border Gateway Protocol (BGP)
routing information from multiple locations around the world. By continuously gathering BGP data from
a network of route collectors (RRCs) RIS helps operators debug network issues, and supports research
into the structure and dynamics of the Internet’s routing system.

The main documentation website is at [https://ris.ripe.net/docs/mrt/](https://ris.ripe.net/docs/mrt/).

The naming pattern for the files is as follows: `https://data.ris.ripe.net/rrcXX/YYYY.MM/TYPE.YYYYMMDD.HHmm.gz`
with:

* `XX`: The collector number
* `YYYY`: year
* `MM`: month
* `TYPE`: the type of file, which is either `bview` (dumps) or `update` (updates)
* `DD`: day
* `HH`: hours
* `mm`: minutes

Dumps are created every 8 hours, and updates are created every 5 minutes

*Per peer metadata* for peers where we have location information is in the
[RIS peer metadata files](https://ris.ripe.net/docs/prototypes/peer-metadata/): [metadata_latest.json](https://www.ris.ripe.net/prototypes/peer-metadata/metadata_latest.json).
This includes the *location* for peers where this information is available. The
`country` and `city` reference the UN locode ([primary source](https://unece.org/trade/uncefact/unlocode),
[alternative dataset](https://github.com/datasets/un-locode)) of the location of the peer.

The location information is mostly available for `multihop` peers. In other
situations the location can be derived using other methods (for example the IP
range of IXP peering VLANs).

## RIPE RIS collectors

The authoritative list of route collectors is [in the documentation site](https://ris.ripe.net/docs/route-collectors/).
Each route collector is either a physical machine or a virtual machine that
transfers BGP updates to the main RIS infrastructure.

Many route collectors are located at an internet exchange. The location of a
route collector **does not necesarily** imply the location of the BGP peer.

There are three main group of peers:

* *local peers*: peers that RIS connects to via an IXP or local connection.
* *route servers*: IXP route servers that peer with RIS. Depending on the IXP,
  the route server AS may or may not be present in the AS path.
* *multihop*: A BGP session over the internet.

## Changelog

The internet is dynamic and so are the RIS peers. This means that a change in
the set of peers is to be expected. However some events are relevant for others
that consume the data. We track some of these events below.

Below we try to keep track of major changes. The list of issues below is a
subset of events that affect data. Please contribute events if you think they
are relevant for other consumers of the data.

#### Ongoing issues:

There is a configuration error with some sessions, where multiple sessions are
configured with the same IP but a different AS number. These may be stable
or unstable sessions. We will adjust these on a case-by-case basis and adjust
this documentation.

#### 2026-02-10:

After the replacement of `rrc12` in the first week of February, the physical
machine for rrc12 was re-connected and started sending messages to your backend.

This injected spurious state change messages into our processing pipeline. BGP
update files for `rrc12` starting from `updates.20260210.1850.gz` up to
`updates.20260210.2145.gz` are affected.

All BGP sessions on `rrc12` were reset and brought back up after 21:45 UTC.
`updates.20260210.2155.gz` and following update files contain the reconnects by
all peers.

#### 2025-04-15:

We de-peered a MIX peer (`217.29.67.xx`) on `rrc10` after a flood of updates.
Update files between ~12:25 and ~13:55 UTC are significantly larger than expected.

#### 2023-08-22: Stuck routes for `2a00:1e68:112::1`

After de-peering `2a00:1e68:112::1` (`AS 42861`) on `rrc00`, some routes from
this peer were stuck in our processing pipeline. We did not detect this early
due to missing consistency checks.

These routes were present in the table dumps (bview files) until 9 May 2025
(first removed in [bview.20250509.1600.g](https://data.ris.ripe.net/rrc00/2025.05/bview.20250509.1600.gz)).
