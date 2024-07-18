# Copy files from cluster dbfs to local computer
Leverage [databricks cli](https://docs.databricks.com/dev-tools/cli/index.html) .
Configure the cli with token and host first.
Then, example command:
```

lin.zhou@C02FX2URMD6R databricks-cli % dbfs ls dbfs:/user/hive/warehouse/stream_table_optimize
lin.zhou@C02FX2URMD6R databricks-cli % mkdir ../universe/delta-standalone/golden-tables/....../stream_table_optimize
lin.zhou@C02FX2URMD6R databricks-cli % dbfs cp -r dbfs:/user/hive/warehouse/stream_table_optimize/ ../universe/delta-standalone/golden-tables/....../stream_table_optimize/

```

# Checkout a PR locally
Take https://github.com/databricks/runtime/pull/84357 as an example, it's from branch `allisonport-db:compile-spark-master`
```
$git remote add allison git@github.com:allisonport-db/runtime.git

$git fetch allison compile-spark-master

$git checkout -b DTS-2966-fetched FETCH_HEAD
```

# Run anlaysis on mysql database for UC db changes
1. Get access with `bin/tshx --env dev kube-login --all`
2. start with `bin/click`
3. Run the following
```
> context staging
> namespace managed-catalog
> pods
```
4. the pick a number, say `0`
5. then `exec bash` to log into the pod
6. then `scripts/mysql_console` to enter mysql

Then you can run `explain + query`!

# Trick to get Permissions on tables after genie
1. Go to the table you need permission
2. Try grant a random permission to a random user
3. Right click on the screen, click "Inspect", go to "Network" tab , find the 2nd from the last request, right click, select "Copy" -> "Copy as fetch".
4. revok the permission in step 2.
5. Go to "Concole" tab, tap in "allow pasting", and then paste the copied text.
6. change the principle to "linzhou+dbadmin@databricks.com", change the link to ".../catalogs/<catalog_name>", change the permission to `\"USE_CATALOG\",\"USE_SCHEMA\",\"SELECT\"`
7. revoke the permission after debugging.

   An example of the text:
```
fetch("https://lne-dev-concerts.cloud.databricks.com/ajax-api/2.1/unity-catalog/permissions/catalog/shr_mkt_measurement", {
  "headers": {
    "accept": "application/json, text/plain, */*",
    "accept-language": "en-US,en;q=0.9",
    "content-type": "application/json",
    "priority": "u=1, i",
    "sec-ch-ua": "\"Not/A)Brand\";v=\"8\", \"Chromium\";v=\"126\", \"Google Chrome\";v=\"126\"",
    "sec-ch-ua-mobile": "?0",
    "sec-ch-ua-platform": "\"macOS\"",
    "sec-fetch-dest": "empty",
    "sec-fetch-mode": "cors",
    "sec-fetch-site": "same-origin",
    "x-csrf-token": "7ea94df3-c1bb-486c-8193-4622caf64ca7",
    "x-databricks-attribution-tags": "%7B%22loadedUiVersions%22%3A%7B%22dbsql%22%3A%22a5d0fce8d3eab45555d45d04f015302800fcb5d5%22%2C%22monolith%22%3A%22b2118b125c36172bfda5879343bf02e08c3883bd%22%7D%2C%22clientBranchName%22%3A%22DEPRECATED%22%2C%22browserIdleTime%22%3A387.70000000298023%2C%22browserTabId%22%3A%22c8571f70-7749-4a38-aa2a-f9d6eb052340%22%2C%22browserHasFocus%22%3Atrue%2C%22browserIsHidden%22%3Afalse%2C%22browserHash%22%3A%22%22%2C%22browserPathName%22%3A%22%2Fexplore%2Fdata%2Fshared_with_concerts%2Fstg%2Ffacebook_campaign_performance_v1%22%2C%22browserHostName%22%3A%22lne-dev-concerts.cloud.databricks.com%22%2C%22browserUserAgent%22%3A%22Mozilla%2F5.0%20(Macintosh%3B%20Intel%20Mac%20OS%20X%2010_15_7)%20AppleWebKit%2F537.36%20(KHTML%2C%20like%20Gecko)%20Chrome%2F126.0.0.0%20Safari%2F537.36%22%2C%22eventWindowTime%22%3A115796.39999999106%2C%22clientTimestamp%22%3A1720668967518%2C%22clientLocale%22%3A%22en%22%2C%22browserLanguage%22%3A%22en-US%22%2C%22queryParameters%22%3A%22%3Fo%3D2137768870322154%26tableDetailsTab%3Dpermissions%22%2C%22pageId%22%3A%22discovery.data_explorer.data.table%22%2C%22pageViewId%22%3A%221720668874790h5d4mz7j%22%7D",
    "x-databricks-org-id": "2137768870322154"
  },
  "referrer": "https://lne-dev-concerts.cloud.databricks.com/explore/data/shared_with_concerts/stg/facebook_campaign_performance_v1?o=2137768870322154&tableDetailsTab=permissions",
  "referrerPolicy": "strict-origin-when-cross-origin",
  "body": "{\"changes\":[{\"principal\":\"lin.zhou+dbadmin@databricks.com\",\"remove\":[],\"add\":[\"USE_CATALOG\",\"USE_SCHEMA\",\"SELECT\"]}]}",
  "method": "PATCH",
  "mode": "cors",
  "credentials": "include"
});
```

# Speed up runtime test 
```
SCALA_INCREMENTAL=true bazel run //sql/core:io.delta.sharing.spark.DeltaSharingDataSourceDeltaSuite
```

