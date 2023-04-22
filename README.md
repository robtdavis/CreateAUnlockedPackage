
# Steps to Create Unlocked Package

1. Clone Repo to local machine
    ```
     git clone https://github.com/robtdavis/CreateAUnlockedPackage.git 
     ```
2. Setup Dev Hub: https://help.salesforce.com/s/articleView?id=sf.sfdx_setup_enable_devhub.htm&type=5

3. Enable Unlocked Packages and Second-Generation Managed Packages : https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_setup_enable_secondgen_pkg.htm

4. Authorize Dev Hub for Project
    ```
    sfdx force:auth:web:login -d -a "DevHub"
    ```
5. Packages use the sfdx-project.json as the root of all package data
    
    ```
    {
        "packageDirectories": [
            {
            "path": "./core",
            "default": false
            },
            {
            "path": "./permissions",
            "default": true
            }
        ],
        "name": "CreateFirstUnlockedPackage",
        "namespace": "",
        "sfdcLoginUrl": "https://login.salesforce.com",
        "sourceApiVersion": "56.0"
    }
    ```
6. Can push to a scratch org from this repo and if you pull it will come back to the path that has the Package Directory with the Default property marked as true.

7. First we need to create a package named "Core" :

    ```
    sfdx package:create --name "Core" --package-type Unlocked --path "./core" --description "Core fields and Objects" 
    ```
8. You should get a Package Id returned starting with 0Ho and the sfdx-project.json will be updated to show the new Package Id.

9. We will need to create a version of the package :

    ```
    sfdx package:version:create --package "Core" --installation-key-bypass --wait 10 --target-dev-hub DevHub --code-coverage
    ````
10. Will get a response that it is creating the version. Will take a few minutes and return with a response similar to :

    ```
    Version create.... Create version status: Success
    Successfully created the package version [08cDp000000CacMIAS]. Subscriber Package Version Id: 04tDp000000IXhoIAG     
    Package Installation URL: https://login.salesforce.com/packaging/installPackage.apexp?p0=04tDp000000IXhoIAG
    As an alternative, you can use the "sfdx package:install" command.
    ```
11. You have now created a Beta Package.

12. The sfdx-project.json will be updated to show the new version package id.
    ```
    {
        "packageDirectories": [
            {
            "path": "./core",
            "default": false,
            "package": "Core",
            "versionName": "ver 0.1",
            "versionNumber": "0.1.0.NEXT",
            "versionDescription": "Core fields and Objects"
            },
            {
            "path": "./permissions",
            "default": true
            }
        ],
        "name": "CreateFirstUnlockedPackage",
        "namespace": "",
        "sfdcLoginUrl": "https://login.salesforce.com",
        "sourceApiVersion": "56.0",
        "packageAliases": {
            "Core": "0HoDp0000004CHOKA2",
            "Core@0.1.0-1": "04tDp000000IXhoIAG"
        }
    }
    ```
13. Useful command to view the packages in your Dev Hub:
    ```
    sfdx package:version:list
    ```
    Shows if the package is a beta or release package

14. Shows all the ids associated with packages in your Dev Hub:
    ```
    sfdx package:create:version:list
    ```
15. To create the Released Package:
    ```
    sfdx package:version:promote --package "Core@1.0.0-1"
    ```
16. To view a individual report on your package:
    ```
    sfdx force:package:version:report --package "Core@1.0.0-1"
    ```
17. To install in org from command line:

    Create scratch org:
    ```
    sfdx force:org:create -s -f config/project-scratch-def.json
    ```
    Install package
    ```
    sfdx package:install --package "Core@1.0.0-1" --target-org test-cjplqklpgj8s4@example.com  --wait 10 --publish-wait 10
    ```




# Salesforce DX Project: Next Steps

Now that you’ve created a Salesforce DX project, what’s next? Here are some documentation resources to get you started.

## How Do You Plan to Deploy Your Changes?

Do you want to deploy a set of changes, or create a self-contained application? Choose a [development model](https://developer.salesforce.com/tools/vscode/en/user-guide/development-models).

## Configure Your Salesforce DX Project

The `sfdx-project.json` file contains useful configuration information for your project. See [Salesforce DX Project Configuration](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_config.htm) in the _Salesforce DX Developer Guide_ for details about this file.

## Read All About It

- [Salesforce Extensions Documentation](https://developer.salesforce.com/tools/vscode/)
- [Salesforce CLI Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
- [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm)
- [Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference.htm)
