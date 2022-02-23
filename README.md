![](images/IBM_Software_Everywhere.png)
# Software Everywhere Turbonomics BEGINNER Tutorial

Hello! Welcome to Caroline Ehler's documentation/tutorial I've compiled for the Software Everywhere project!
This tutorial is specific to Turbonomic.

## Installation
See below for complete tutorial.
## Usage

## Tutorial
### Set-Up
To begin, you'll need to be sure to have all of these things installed on your machine: Docker & Node.js

If you don't, follow these steps:

A. Download Homebrew: https://sourabhbajaj.com/mac-setup/Homebrew/
Follow the Homebrew setup tutorial.

B. Download Homebrew's Node.js: https://sourabhbajaj.com/mac-setup/Node.js/

C. Download Homebrew's Docker: https://sourabhbajaj.com/mac-setup/Docker/
BUT I find this tutorial a little confusing. Here is what to do:
```bash
brew install cask
brew install --cask docker
```

D. If you don't have an IDE, I'm going to be using VS Code, so be sure to download that too.

### Begin Tutorial
1. Create Cluster
2. Clone the Iascable Repo here: https://github.com/cloud-native-toolkit/iascable
```bash
git clone https://github.com/cloud-native-toolkit/iascable.git
```


4. Download this Bill of Materials (BOM1): https://github.com/cloud-native-toolkit/automation-solutions/blob/main/boms/turbonomic/turbo-gitops-bom.yaml 
5. go to iascable directory
6. Build Turbonomics Bill of Materials (BOM) with iascable tool
```bash 
./iascable build -i <location of BOM1 yaml> -o <location for output>
```
Note: the location should be a path
6. Within the location of output from above, edit the resulting tfvars file with your environment info for your cluster
7. Set your ibmapikey first
8. Run the automation with a docker terraform container 
```bash
docker run -it -e TF_VAR_ibmcloud_api_key=$APIKEY -e IBMCLOUD_API_KEY=$APIKEY -v ${PWD}:/terraform -w /terraform quay.io/ibmgaragecloud/cli-tools:v0.15
```


/*
5.  Pull this file down: https://github.com/cloud-native-toolkit/automation-solutions/blob/main/boms/turbonomic/turbo-gitops-bom.yaml 
6. 
4. Create a Bill of Materials (BOM) called ```gitopsbootstrap-bom.yaml``` ?where?

```python
apiVersion: cloud.ibm.com/v1alpha1
kind: BillOfMaterial
metadata:
  name: multicloud-cluster
spec:
  modules:
    - name: ocp-login
    - name: olm
    - name: gitops-namespace
    - name: argocd-bootstrap
    - name: gitops-console-link-job
    - name: gitops-cluster-config
      alias: config
      variables:
        - name: banner_text
          important: true
  ```
  */
## Contributing




