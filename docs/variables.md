# AVD Variables and Data Model

The Arista Validated Designs (AVD) framework relies on YAML-based variable definitions to describe every aspect of the network, from topology and addressing to features such as VLANs, VRFs, and BGP EVPN. The AVD data model also includes endpoint configuration and port profile definitions.

These variables are organized primarily under the **`group_vars/`** and **`host_vars/`** directories and serve as the source of truth for configuration generation.

---
