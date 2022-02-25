![](images/IBM_Software_Everywhere.png)
# Software Everywhere Turbonomics BEGINNER Set-Up Tutorial


Hello! Welcome to Caroline Ehler's documentation/tutorial I've compiled for the Software Everywhere project!
This tutorial is specific to Turbonomic.

**Contact**
Should there be any issues with this tutorial, please contact caroline.ehler@ibm.com. Thanks!

## Tutorial
### Set-Up
To begin, you'll need to be sure to have all of these things installed on your machine: Docker & Node.js

If you don't, follow these steps:

A. Download Homebrew: https://sourabhbajaj.com/mac-setup/Homebrew/
Follow the Homebrew setup tutorial.

B. Download Homebrew's Node.js: https://sourabhbajaj.com/mac-setup/Node.js/

C. Download Homebrew's Docker: https://sourabhbajaj.com/mac-setup/Docker/

From the Homebrew tutorial:

Docker for Mac can be downloaded here: https://docs.docker.com/desktop/mac/install/
```bash
brew install cask
brew install --cask docker
```

### Begin Tutorial
1. Create a cluster on Red Hat Openshift using IBM Cloud. https://cloud.ibm.com/ 

<img src="images/ibmcloudss.png" width="500">

2. Clone the iascable repo here: https://github.com/cloud-native-toolkit/iascable
```bash
git clone https://github.com/cloud-native-toolkit/iascable.git
```

3. Inside the iascable directory, install node.js
```bash 
npm install
```

4. Create a Bill of Materials (BOM) called ```gitopsbootstrap-bom.yaml``` in iascalbe.

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
  
  5. Run the BOM to generate the terraform file: 
 
  ./iascable build -i <location_of_gitopsbootstrap-bom.yaml_file> -o <location_for_output>
```bash 
./iascable build -i gitopsbootstrap-bom.yaml -o ./myFolder
```

6. Accessing the generated folder from the above command, cd into myFolder --> multicloud-cluster --> terraform

7. Edit the file ```multicloud-cluster.auto.tfvars```. Uncomment the last line of each section to add your modifications. 

7a. Banner Text: The title of the top banner in the cluster

```config_banner_text="Turbonomics Tutorial"```

7b. Namespace Name = The value that should be used for the namespace

```namespace_name="gitops-tools"```

7c. Server URL: The url for the OpenShift api

```server_url="https://c100-e.us-east.containers.cloud.ibm.com:31361"```

To access this, go to the OpenShift console from your cluster. 

![](images/osconsole.png)

Click the dropdown from your username.

Click "Copy login command".

![](images/loginos.png)

Hit display token.

Use the URL that follows ```--server=``` from the Login with this Token ```oc login``` line.

![](images/token.png)

7d. Cluster Login Token:

```cluster_login_token="sha256....."```

Following the same steps as above for the Server URL, go to the same page with your OpenShift token login. 

Use the API token as the Cluster Login Token.
**NOTE: The Cluster Login Token will time-out after about an hour.** You will have to modify the terraform file again if you cannot get to step 9 within an hour of generating this Login Token.

7e. Gitops-repo_host: The host for the git repository. (Use github.com)

```gitops-repo_host="github.com"```

7f. Gitops-repo_type: The type of the hosted git repository (github or gitlab).

```gitops-repo_type="github"```

7g. Gitops-repo_org: The org/group where the git repository exists/will be provisioned. (Your Github username)

```gitops-repo_org="cee7zm"```

7h. Gitops-repo_repo: The short name of the repository (i.e. the part after the org/group name) (The name for the repo the terraform will generate. Be sure the name you choose is not one of your existing repositories already).

```gitops-repo_repo="my_turbo_repo"```

7i. Fitops-repo_username: The username of the user with access to the repository (your github username)

```gitops-repo_username="cee7zm"```

7j. Gitops-repo_token: The personal access token used to access the repository

```gitops-repo_token="...[your generated token]..."```

To access your Github-generated token, go to Github.com.

Login, and select your profile menu. 

<img src="images/settingsmenu.png" width="300">

Go to Settings --> Developer Settings (bottom of left-side menu list) --> Personal Access Tokens (again, bottom of left-side menu)

Generate a new token. **MAKE NOTE OF THIS TOKEN. IT WILL DISAPPEAR AFTER YOU VIEW IT.**

Select the permissions for the token to be at least the bolded repo and delete_repo boxes. 

Hit Generate Token. MAKE NOTE OF THIS TOKEN. Hit the copy button and store it somewhere for future reference.

![](images/terraform.png)


8. Open up Docker to run. In the terraform directory, run the following commands. 

Set the environment variable GITTOKEN to your generated Github token.

```export GITTOKEN="3792a189....." ```

```docker run -it -e TF_VAR_gitops-repo_token=$GITTOKEN -v ${PWD}:/terraform -w /terraform quay.io/ibmgaragecloud/cli-tools:v0.15```

Terraform will begin running. 

```$ terraform init``` This will take a minute to run.

```$ terraform apply -auto-approve```
This will take 10-15 minutes to compelte if this is your first-time running this setup. 

10. Check to see if your OpenShift console app options added Argo like this:

![](images/consolebefore.png)
![](images/consoleafter.png)

## That's it! You've completed the Beginner Turbonomics Set-Up Tutorial!
