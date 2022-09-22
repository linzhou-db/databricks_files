# Instructions

- how to test python: https://github.com/delta-io/delta-sharing#python-connector-1
- how to test scala: https://github.com/delta-io/delta-sharing#apache-spark-connector-and-delta-sharing-server
- to run the spark connector test: `build/sbt spark/test`
- to run the server test: `build/sbt server/test`
- to test a single test case: `build/sbt  'test:testOnly *DeltaSharingSuite -- -z "table1"'`

- The above commands won’t run the integration tests
  - you need to run `source ~/delta_sharing_oss_env.config` to set up the environment.
  - you also need to set up the gcs creds, run `export GOOGLE_APPLICATION_CREDENTIALS=/Users/lin.zhou/delta_sharing_oss_gcp_creds.config

- sbt also supports run only a few tests in a test suite, you can check examples here: https://github.com/delta-io/delta#building
