# Git
### Pull from HEAD: g4 sync
```
git pull <remote> <branch>:
  git pull origin SC-80500
  git pull databricks origin
```

### Reset code to a previous version
```
git log -3
git reset --hard <commit hash>
```

### Add code to PR
```
git add <file>: gid add .
git commit -m “original comment”
git push origin HEAD
```
- Then go to go/u, use UI to create PR

- Update code in existing PR
```
git commit -am “new comment”
git push origin HEAD
```

### Force push
```
git push -uf origin HEAD
```

### Create a New Branch
```
git fetch databricks master
git checkout -b SC-#### databricks/master
```

### Delete Branch
```
git branch -D <branch>
```

### Fetch from a different branch
```
git fetch databricks dbr-branch-9.x
#or 
git fetch databricks dbr-branch-10.x
```
- Get a local branch from a different branch 
```
git checkout -b sc-80500-10.x databricks/dbr-branch-10.x
```

### Check-pick a commit
```
git cherry-pick <commit hash>
```

### Revert changes made to your working copy, do this:
```
git checkout .
```

- If you want to revert changes made to the index (i.e., that you have added), do this. Warning this will reset all of your unpushed commits to master!:
```
git reset
```

- If you want to revert a change that you have committed, do this:
```
git revert <commit 1> <commit 2>
```

- If you want to remove untracked files (e.g., new files, generated files):
```
git clean -f
```
- Or untracked directories (e.g., new or automatically generated directories):
```
git clean -fd
```

# PR Dev
### Run a universe test
```
bazel run path:test -- -z “test name”
```

### Run a runtime test
```
build/sbt 'sql/testOnly *SessionStateSuite -- -z "custom string"'  
```
And usually need to run the command below to export JAVA_HOME
```
source ~/.bashrc
```
Content in .bashrc:
```
# JAVA (sbt related)
export JAVA_HOME=$(/usr/libexec/java_home -v 1.8.0)

export UNIVERSE_ROOT=~/universe
export PATH=$PATH:$UNIVERSE_ROOT/experimental/bin
```

### Find test target 
```
bazel query //managed-catalog/... | grep ShareRpcSuite
bazel query //spark/... | grep ManagedCatalogClientSuite
bazel query //sql/... | grep DeltaSharingCommandsSuite
```

### Format Scala 
```
bin/scalafmt path/to/file
```

### Build Tess Shard from PR
Reference: https://databricks.atlassian.net/wiki/spaces/UN/pages/794232213/Test+Shard+go+testshard+go+help-testshard#TestShard(go%2Ftestshard%2Cgo%2Fhelp-testshard)-viaJenkins
```
jenkins build-aws-cons
```

### Build Test Shard from universe/
- First, get access to dev kubernetes
```
bin/get-kube-access dev
```
- Second, open the link promoted in the command, click 'wish'

- Third, run the following command
```
./bin/testshard create --context dev-aws-us-east-1 --shard-name test-shard-lin-zhou --feature-tier 7
```

### Kubernetes Commands 
- First, get access to kubernetes, dev or staging or prod
```
bin/get-kube-access dev
bin/get-kube-access staging
```
-- If you want to run `exec -it` then need to select "central kubernetes and services - write access", and need a ES-xxx Jira ticket to justify the access.

- Second, set context and namespace, and run other commands as needed:
```
kubectl config set-context staging-aws-us-west-2 --namespace=managed-catalog

kubectl get pods | grep managed-catalog
kubectl logs <pod-name>
kubectl exec -it <pod-name> bash
```

### Debug DeltaSharing Database
After loging into pods, run some scripts under scripts or sqlscripts.

### Custom DBR Image

```
1. (optional) Update WORKSPACE file in universe to use the local repo. Comment out `#repo_archive` and specify local_repository to be:

local_repository(
  name = "workspace_spark_3_2",
  path = "../runtime",
)

2. run `bazel run //spark/images:9.x-snapshot-scala2.12_upload_image`

3. Go to cluster creation page, in js console do `window.prefs.set("enableCustomSparkVersions", true)`

4. Select your uploaded image. Remember to apply the `custom:` prefix.
```


