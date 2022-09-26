# Copy files from cluster dbfs to local computer
Leverage [databricks cli](https://docs.databricks.com/dev-tools/cli/index.html) .
Configure the cli with token and host first.
Then, example command:
```

lin.zhou@C02FX2URMD6R databricks-cli % dbfs ls dbfs:/user/hive/warehouse/stream_table_optimize
lin.zhou@C02FX2URMD6R databricks-cli % mkdir ../universe/delta-standalone/golden-tables/....../stream_table_optimize
lin.zhou@C02FX2URMD6R databricks-cli % dbfs cp -r dbfs:/user/hive/warehouse/stream_table_optimize/ ../universe/delta-standalone/golden-tables/....../stream_table_optimize/

```
