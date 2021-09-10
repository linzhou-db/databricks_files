# DUST
### Access DUST Pre-Provisioned Workspace
First follow necessary steps on "Setting up Databricks CLI"
```
$./venv/bin/databricks --profile dust configure --host https://dbc-07e4ca03-cc44.staging.cloud.databricks.com --token
```
TOKEN is dapicdc6858c7124e2da5743f2390cd3bffc, valid for 90 days starting 09/09/2021

```
$alias uc="$(pwd)/venv/bin/databricks --profile dust unity-catalog"
```

Then you can run the command below to check available cli
```
$uc 
```
