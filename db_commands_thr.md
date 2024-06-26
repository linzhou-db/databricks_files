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
