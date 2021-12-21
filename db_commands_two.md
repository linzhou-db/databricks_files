# DUST
### Access DUST Pre-Provisioned Workspace
First follow necessary steps on ["Setting up Databricks CLI"](https://databricks.atlassian.net/wiki/spaces/UN/pages/2285109449/Testing+Unity+Catalog+in+Staging+go+uc+staging#[inlineExtension]Setting-up-Databricks-CLI) under go/uc/staging
```
$./venv/bin/databricks --profile dust configure --host https://dbc-07e4ca03-cc44.staging.cloud.databricks.com --token
```
TOKEN needs to be required, valid for 90 days starting 09/09/2021

```
$alias uc="$(pwd)/venv/bin/databricks --profile dust unity-catalog"
```

Then you can run the command below to check available cli
```
$uc 
```

# Delta Sharing CLI
Clone the git repository and checkout the branch
```
~/% git clone git@github.com:adamcain-db/databricks-cli.git
~/% cd databricks-cli
databricks-cli/% git checkout SC57404-add-managed-catalog-cli 
```

There's no PR review needed, changes are merged after committing and pushing.

# Test Delta Sharing CLI Local Changes
For example, if dtabricks-cli from SC57404-add-managed-catalog-cli branch is checked out to /Users/lin.zhou/databricks-cli, and local changes are made

This is updated to https://github.com/databricks/databricks-cli-private by Dec 2021. 

- update python version as needed: python3.9 or python2.7, etc.
```
virtualenv venv
. venv/bin/activate
pip install -r tox-requirements.txt
pip install 'tabulate>=0.7.7'
pip install 'click>=6.7'
export PYTHONPATH=/Users/lin.zhou/databricks-cli-private:/Users/lin.zhou/databricks-cli-private/venv/lib/python3.9/site-packages 
pip install -e ./
```

Then you can run and other commands in the [section](https://databricks.atlassian.net/wiki/spaces/UN/pages/2285109449/Testing+Unity+Catalog+in+Staging+go+uc+staging#[inlineExtension]Setting-up-Databricks-CLI)
```
$databricks
```

# Delta Sharing OSS Repository
To get a local copy of the repository:
```
git clone https://github.com/delta-io/delta-sharing.git
```

To check the local repository details
```
git remote -v
```

To check the status of a branch:
```
git status
```

To checkout a branch:
```
git checkout -b SC-88052 -t origin/main
```



