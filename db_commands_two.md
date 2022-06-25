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
Create a PR:
```
git checkout -b local-branch-name
git add .
git commit -m "message"
git push origin HEAD
```

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

# Delta Sharing OSS Local Jar
build jar from local files
```
build/sbt spark/publishM2
```
Remove cache jars
```
rm -R /Users/lin.zhou/.ivy2/cache/io.delta/delta-sharing-spark_2.12 
rm  /Users/lin.zhou/.ivy2/jars/io.delta_delta-sharing-spark_2.12-0.5.0-SNAPSHOT.jar
rm -R /Users/lin.zhou/.m2/repository/io/delta/delta-sharing-spark_2.12/0.5.0-SNAPSHOT/
```

Debug local python changes:
```
# install pyspark
pip install pyspark
​
# publish the spark connector to ~/.m2
build/sbt spark/publishM2
​
# install python connector
cd python/
pip install -e .
​
# Start python cli
python
```

# DBR Custom Image With Local Changes
1. Make code changes in runtime/
2. commit and get the commit hash
3. update universe/spark/versions/3.4/versions.bzl (as of 2022/06/15)
4. in universe/ run  bazel run //spark/images:12.x-snapshot-scala2.12_upload_image (as of 2022/06/15)

# Custom SQL Endpoint/SQL Warehouse
First, create a random sql endpoint from the workspace, then get the warehouse id, warehouses/<warehouse id>/
```
$ TOKEN=<PAT>
$ WORKSPACE_URL=https://e2-dogfood-unity-catalog-us-east-1.staging.cloud.databricks.com

$ curl -X POST -H "Authentication: Bearer $TOKEN" \
-H 'X-Databricks-Allow-Internal: true' \
"$WORKSPACE_URL/api/2.0/sql/warehouses/1a467c732b86babc/edit?debug=true" \
-d '{ "confs": { "test_overrides": { "runtime_version": "custom:custom-local__12.x-snapshot-scala2.12__unknown__sc-101877__2572faa__66ab7de__lin.zhou__2633d8a__format-2.lz4" } } }'
```
