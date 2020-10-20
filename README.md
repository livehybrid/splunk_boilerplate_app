# Splunk Build Pipeline Example

## Introduction
This is an example of how you might build your Splunk apps using a CI/CD pipeline.  
The repo is an extract of a sample app used internally to our organisation to build 100's of apps for deployment to SplunkCloud.  
If you haven't yet seen the Splunk talk "DEV1182C - How to build high quality Splunk vetted apps efficiently with automated builds to streamline your deployments" then it is definately worth watching!

We're using a Gitlab CI/CD pipeline which automatically tests the splunk apps with app inspect, outputs the results, and packages everything for submission to Splunk and can be used as a foundation to run a multitude of tests and processes against your apps as part of the build.

Additional configuration is required to get this working. Details below.

*** Huge credit to Ryan Faircloth for his work from  https://bitbucket.org/SPLServices/buildtools, this saved us so much time when we started our Splunk Cloud journey!


## Quick Start

```mkdir your_app_id
cd your_app_id
git init
git remote add bootstrap https://github.com/livehybrid/splunk_boilerplate_app.git
git pull bootstrap master
git remote rm bootstrap
git remote add origin <Your apps git repo>
```

### config.mk
Edit the config.mk file and update the fields `MAIN_APP` , `AUTHOR`, `MAIN_DESCRIPTION`, `MAIN_LABEL`, for example:

```
MAIN_APP    = org_all_indexer_base
#Name of the license file in the root of the repo
LICENSE_FILE    = license-eula.txt
LICENSE_URL     = https://www.splunk.com/en_us/legal/splunk-software-license-agreement.html

AUTHOR      = William Searle
COMPANY     = LiveHybrid Ltd

MAIN_DESCRIPTION = App containing all the necessary base config for our indexers
MAIN_LABEL = Indexer Base Config
```

### Drop in / create your App
Create or move your app into the '<your_app_id>/src/<your_app_id>' directory

### app.manifest
Copy the `app.manifest.example` file into `<your_app_id>/src/<your_app_id>` directory.  

The schema including detailed information on the specifics with an app.manifest file can be found at https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit/pkgtoolkitref/pkgtoolkitapp/

```
{
  "schemaVersion": "1.0.0",
  "info": {
    "title": "APP TITLE GOES HERE",
    "id": {
      "group": null,
      "name": "APP ID GOES HERE",
      "version": "1.0.0"
    },
    "author": [
      {
        "name": "YOUR NAME GOES HERE",
        "email": null,
        "company": null
      }
    ],
    "releaseDate": null,
    "description": "",
    "classification": {
      "intendedAudience": null,
      "categories": [],
      "developmentStatus": null
    },
    "commonInformationModels": null,
    "license": {
      "name": null,
      "text": null,
      "uri": null
    },
    "privacyPolicy": {
      "name": null,
      "text": null,
      "uri": null
    },
    "releaseNotes": {
      "name": null,
      "text": null,
      "uri": null
    }
  },
  "dependencies": {

  },
  "tasks": [],
  "inputGroups": {
  },
  "incompatibleApps": {
  },
  "platformRequirements": {
    "splunk": {
      "Enterprise": "*"
    }
  }
}
```

### Push & Commit

This should trigger a standard package build and run your pipeline.


```
cd ..
git add *
git commit -m "COMMENT GOES HERE"
git push --set-upstream origin master
```

### Inspect results
Inspect your app inspect output and adjust accordingly.

### Tag & Release
Create a new tag - either through the Gitlab UI or git tag 0.0.1 && git push --tags (Where 0.0.1 is the tag)

This will trigger a build and release

Navigate to your app's Release page https://git.<yourGitLabInstance>/YOURAPP_ID/releases 
This might take a few minutes to get everything packaged up.