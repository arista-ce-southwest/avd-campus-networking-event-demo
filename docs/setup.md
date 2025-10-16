# AVD Environment Setup

Ensure your environment meets the following requirements:

- **Python 3.12+**
- **Ansible 2.14+**
- **Arista AVD Collection 5.4+**
- **CloudVision as a Service** (CVaaS) subscription
- **Virtual Environment (venv)** for Python dependency isolation

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

1. Validate the inventory structure:

```bash
ansible-inventory -i inventory.yml --graph
ansible-inventory -i inventory.yml --list
```

1. Create a service account in CloudVision (CVaaS):

[Steps to create service accounts on CloudVision - AVD==5.4](https://avd.arista.com/5.4/ansible_collections/arista/avd/roles/cv_deploy/index.html#steps-to-create-service-accounts-on-cloudvision)
