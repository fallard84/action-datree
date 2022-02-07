# Overview
The Datree CLI provides a policy enforcement solution to run automatic checks for rule violations in Kuberenetes configuration files.  
This action runs the Datree CLI against given k8s configuration file/s in your repository.<br/>
To learn more about Datree, visit the [datree website](https://www.datree.io/).
<br/><br/>
# Setup
To get started, you will need to obtain your Datree account token. Follow the simple instructions described [here](https://hub.datree.io/account-token).
<br/><br/>
Then, configure your token:
* Set DATREE_TOKEN as a [secret](https://docs.github.com/en/actions/reference/encrypted-secrets) or [environment](https://docs.github.com/en/actions/reference/environment-variables) variable (see example at the bottom of this readme).  
<br/><br/>
# Usage
In your workflow, set this action as a step:
```
- name: Run Datree's CLI
        uses: datreeio/action-datree@main
        with:
          file: 'someDirectory/someFile.yaml'
          options: '--output simple --schema-version 1.20.0'
```
**file** (**required**) - a path to the file/s you wish to run your Datree test against. This can be a single file or a [Glob pattern](https://www.digitalocean.com/community/tools/glob) signifying a directory.  
**options** (**optional**) - the desired [Datree CLI arguments](https://hub.datree.io/cli-arguments) for the policy check. In the above example, two of these arguments(--output and --schema-version) are used.  
<br/><br/>
# Examples
Here is an example workflow that uses this action to run a Datree policy check on all of the .yaml files under the current directory, on every push/pull request:
```yaml
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
    
env:
  DATREE_TOKEN: ${{ secrets.DATREE_TOKEN }} 

jobs:
  k8s-policy-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2
        
      - name: Run Datree's CLI
        uses: datreeio/action-datree@main
        with:
          file: '**/*.yaml'
          options: ''
```
<br/>

# Output
The result of your policy checks will look like this:  

![](/Resources/output.gif)
