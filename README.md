# Campus Deployment & Operations for Modern Networking

## Arista Network Automation - NaaS

This repository contains the full AVD (Arista Validated Design) configuration and automation workflows for a modern campus network deployment demo. The workflow uses CVaaS (CloudVision as a Service), GitHub CI/CD, and Ansible to automate configuration deployment to lab devices.

### Workflow Diagram

```mermaid
sequenceDiagram
  participant User
  participant AVD
  participant GH as GitHub
  participant CVaaS
  participant Device
  
  par Offline Configuration Prep
    User->>AVD: Render configuration using AVD framework
  end
  par Automation Workflow
    AVD->>GH: Commit render output
    GH->>AVD: Trigger build.yml
    AVD->>+CVaaS: Execute deploy-studio.yml
    CVaaS->>Device: Push configuration to device
    Device-->>CVaaS: Acknowledge deployment
    CVaaS-->>AVD: Deployment status
  end
  
```

## Required Software Versions

Ensure your environment meets the following requirements:

* Python 3.12 or Newer
* Ansible: 2.14+
* Arista AVD Collection: 5.4+
* Virtual Environment: venv for Python dependency isolation
* CVaaS: Active subscription for device orchestration

## Clone Git Respository

Clone the repository into a local working directory:

```bash
git clone git@github.com:arista-ce-southwest/avd-campus-networking-event-demo.git
cd avd-gns3-ztp-cvaas-demo
```

## Setup Python Virtual Environment

Activate the virtual environment included in the repository:

```bash
python3 -m venv venv
source venv/bin/activate
```

Validate that required packages are installed:

```bash
pip install --upgrade pip
pip install -r requirements.txt
ansible --version
```

## Inventory Validation and Service Account Creation in CVP

AVD uses an Ansible inventory to define devices, VRFs, VLANs, and SVIs for the demo network.

1. Validate the inventory:

```bash
ansible-inventory -i inventory.yml --graph
ansible-inventory -i inventory.yml --list
```

2. Create service account in Arista CloudVision

[Steps to create service accounts on CloudVision - AVD==5.4](https://avd.arista.com/5.4/ansible_collections/arista/avd/roles/cv_deploy/index.html#steps-to-create-service-accounts-on-cloudvision)

## Rendering and Deploying Configurations

AVD renders configuration in structured format before deployment:

1. Run the "build" Ansible playbook

```bash
ansible-play -i inventory build.yml
```

2. Run the "deploy-studio" Ansible playbook to push the rendered configuration into CloudVision Studios

```bash
ansible-play -i inventory deploy-studio.yml
```

## Topology Overview

The demo topology includes:

* L2 Leaf: SC-B1-IDF1 (handles inband management VLAN 10, DHCP server endpoint)
* L3 Spine/Core: SC-B1-Core1 / SC-B1-Core2 (DHCP relay, BGP peering, SVI VRF routing)
* Access Devices: APs, PCs, connected via defined VLANs (10, 11, 12)

```mermaid
graph TD
    %% Automation Layer
    subgraph "Automation and Control Plane"
        AVD[AVD Framework - Ansible and GitHub Actions]
        CVaaS[Arista CloudVision as a Service]
    end

    %% Core Layer
    subgraph "Core or Spine Layer"
        Core1[SC-B1-Core1 - L3 Spine, DHCP Relay, BGP, VRF Routing]
        Core2[SC-B1-Core2 - L3 Spine, DHCP Relay, BGP, VRF Routing]
    end

    %% Leaf Layer
    subgraph "Access or Leaf Layer"
        Leaf[SC-B1-IDF1 - L2 Leaf, DHCP Server, VLAN Termination]
    end

    %% Endpoint Layer
    subgraph "Access Devices"
        APs[Access Points - VLANs 10 and 11]
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

* AVD is shown at the top as the automation source generating configurations.
* CVaaS acts as the control and deployment hub for configuration delivery to devices.
* Core switches handle L3 services and DHCP relay, while SC-B1-IDF1 acts as the DHCP server endpoint for VLAN 10.
* Access Devices (APs and PCs) connect through VLANs.


## References

* [Arista AVD Documentation](https://avd.arista.com/)
* [CloudVision as a Service](https://www.arista.com/en/products/cloudvision)
* [Ansible EOS Collection](https://docs.ansible.com/ansible/latest/collections/arista/eos/index.html)
* [MermaidJS Diagrams](https://mermaid.js.org/)
