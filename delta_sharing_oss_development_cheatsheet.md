# Instructions

## Run python ests
https://github.com/delta-io/delta-sharing#python-connector-1

- test a single test case in python

Use `-s` to output debug to console.
```
$python/dev/pytest -s /Users/lin.zhou/delta-sharing/python/delta_sharing/tests/test_delta_sharing.py -k "test_load_as_spark"
```
- To test load_as_spark on latest scala changes, need to release 1.0.0-SNAPSHOT locally, update the `spark.jars.packages` in `test_delta_sharing.py`
There are lots of caches playing around, so make sure to add lots of print debug info to test.
```
$build/sbt spark/publishM2

$ls /Users/lin.zhou/.m2/repository/io/delta/delta-sharing-spark_2.12/

```


## Run spark/scala Tests

- https://github.com/delta-io/delta-sharing#apache-spark-connector-and-delta-sharing-server
- to run the spark connector test: `build/sbt spark/test`
- to run the server test: `build/sbt server/test`
- to test a single test case: `build/sbt  'test:testOnly *DeltaSharingSuite -- -z "table1"'`

## Run integration tests
- The above commands won’t run the integration tests
  - you need to run `source ~/delta_sharing_oss_env.config` to set up the environment.
  - you also need to set up the gcs creds, run `export GOOGLE_APPLICATION_CREDENTIALS=/Users/lin.zhou/delta_sharing_oss_gcp_creds.config

- sbt also supports run only a few tests in a test suite, you can check examples here: https://github.com/delta-io/delta#building
