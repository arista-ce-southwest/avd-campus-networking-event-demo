# Topology Overview

The demo topology includes:

- L2 Leaf: SC-B1-IDF1 (inband management VLAN 10, DHCP server endpoint)
- L3 Spine/Core: SC-B1-Core1 / SC-B1-Core2 (DHCP relay, BGP peering, SVI routing)
- Access Devices: APs and PCs connected via VLANs 10, 11, and 12

```mermaid
graph TD
    %% Automation Layer
    subgraph "Automation and Control Plane"
        AVD[AVD Framework - Ansible and GitHub Actions]
        CVaaS[Arista CloudVision as a Service]
    end

    %% Core Layer
    subgraph "Spine Layer (MLAG)"
        Core1[SC-B1-Core1 - L3 Spine, DHCP Relay, BGP, VRF Routing]
        Core2[SC-B1-Core2 - L3 Spine, DHCP Relay, BGP, VRF Routing]
    end

    %% Leaf Layer
    subgraph "Access or Leaf Layer"
        Leaf[SC-B1-IDF1 - L2 Leaf, DHCP Server, VLAN Termination]
    end

    %% Endpoint Layer
    subgraph "Access Devices"
        APs[Access Points - mgmt-vlan10 and SSID-vlan11]
        PCs[PC Clients - VLAN 12]
    end

    %% Topology Connections
    AVD -->|Render and Deploy Config| CVaaS
    CVaaS -->|Push Configuration| Core1
    Core1 ---|Uplink| Leaf
    Core2 ---|Uplink| Leaf
    Leaf ---|VLAN 10| APs
    Leaf ---|VLAN 11| APs
    Leaf ---|VLAN 12| PCs

    %% DHCP Workflow
    subgraph "Management and DHCP Flow"
        PCs -->|DHCP Request| Leaf
        Core1 -->|DHCP Relay| Leaf
        Core2 -->|DHCP Relay| Leaf
    end
```

**Key Highlights:**

- AVD serves as the automation source for configuration generation.
- CVaaS is the central deployment and orchestration platform.
- Core switches provide L3 services and DHCP relay.
- Access devices (APs and PCs) connect through VLANs defined in the demo fabric
