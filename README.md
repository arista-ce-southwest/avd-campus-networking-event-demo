# Campus Deployment & Operations for Modern Networking

## Arista Network Automation - NaaS
FIXME: Update mermaid sequence diagram
```mermaid
sequenceDiagram
  participant User
  participant GH
  participant AVD
  participant CVaaS
  participant Device
  
  par Device Connection to Physical Network
    User->>+Device: User connects physical device into network
    Device-->>-CVaaS: Registers to CVaaS via ZTP services
  end
  par Configurtion Prep 
    User->>AVD: Render Configuration from AVD Framework
    AVD->>GH: Push Render Configuration into GitHub
  end
  par GitHub Actions CI/CD Workflow
    GH->>AVD: Run gen-inventory.yml
    AVD->>+CVaaS: GET Call for Device Inventory
    CVaaS-->>-AVD: GET Respone: 200
    GH->>AVD: Run gen-l2-structured-services.yml
    GH->>AVD: Run build.yml
    GH->>+AVD: Run deploy-studio.yml
    AVD-->>-CVaaS: Deploy config to CVaaS
    CVaaS->>Device: Push config via change management
  end
```

## AVD Demo Required Software Version

TODO: create list of required softward versions

* Python 3.12 or Newer

## Clone Git Respository 

Clone down the git repository which contains the lab's working code into a working directory 

FIXME: Update git repo url
```bash
git@github.com:arista-ce-southwest/avd-gns3-ztp-cvaas-demo.git
```

## Access the Python Virtual Environment and Validate AVD Services

TODO: create AVD environ for demo

The python virtual environment should be included when the repository is cloned

Activate the environment and validate the Ansible and AVD componets.

1. activate the virtual environment

```bash
$ source venv/bin/activate
(venv) :~$ 
```