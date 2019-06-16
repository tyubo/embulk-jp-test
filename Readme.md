# env

- Mac OS High Sierra 10.13.6
- digdag v0.9.33
- java version "1.8.0_161"

# with digdag
```bash
cd with_digdag
docker build . -t jp_test/with_digdag
digdag run jp_test.dig
```

An error occurred

```
2019-06-09 12:44:07.237 +0000 [INFO] (0001:transaction): Using JDBC Driver PostgreSQL 9.4 JDBC4.1 (build 1205)
org.embulk.exec.PartialExecutionException: org.embulk.config.ConfigException: 'table' or 'query' parameter is required
	at org.embulk.exec.BulkLoader$LoaderState.buildPartialExecuteException(BulkLoader.java:340)
	at org.embulk.exec.BulkLoader.doRun(BulkLoader.java:548)
	at org.embulk.exec.BulkLoader.access$000(BulkLoader.java:35)
	at org.embulk.exec.BulkLoader$1.run(BulkLoader.java:353)
	at org.embulk.exec.BulkLoader$1.run(BulkLoader.java:350)
	at org.embulk.spi.Exec.doWith(Exec.java:22)
	at org.embulk.exec.BulkLoader.run(BulkLoader.java:350)
	at org.embulk.EmbulkEmbed.run(EmbulkEmbed.java:162)
	at org.embulk.EmbulkRunner.runInternal(EmbulkRunner.java:292)
	at org.embulk.EmbulkRunner.run(EmbulkRunner.java:156)
	at org.embulk.cli.EmbulkRun.runSubcommand(EmbulkRun.java:436)
	at org.embulk.cli.EmbulkRun.run(EmbulkRun.java:91)
	at org.embulk.cli.Main.main(Main.java:26)
	Suppressed: java.lang.NullPointerException
		at org.embulk.exec.BulkLoader.doCleanup(BulkLoader.java:445)
		at org.embulk.exec.BulkLoader$3.run(BulkLoader.java:385)
		at org.embulk.exec.BulkLoader$3.run(BulkLoader.java:382)
		at org.embulk.spi.Exec.doWith(Exec.java:22)
		at org.embulk.exec.BulkLoader.cleanup(BulkLoader.java:382)
		at org.embulk.EmbulkEmbed.run(EmbulkEmbed.java:165)
		... 5 more
Caused by: org.embulk.config.ConfigException: 'table' or 'query' parameter is required
	at org.embulk.input.jdbc.AbstractJdbcInputPlugin.getRawQuery(AbstractJdbcInputPlugin.java:382)
	at org.embulk.input.jdbc.AbstractJdbcInputPlugin.setupTask(AbstractJdbcInputPlugin.java:217)
	at org.embulk.input.jdbc.AbstractJdbcInputPlugin.transaction(AbstractJdbcInputPlugin.java:201)
	at org.embulk.exec.BulkLoader.doRun(BulkLoader.java:489)
	... 11 more
```

# embulk only
```bash
cd embulk_only
docker build . -t jp_test/embulk_only
docker run -it jp_test/embulk_only bash -c "embulk run -b /var/lib/embulk jp_test.yml.liquid"
```

Test was successful
```
2019-06-09 12:47:33.001 +0000 [INFO] (0001:transaction): Using JDBC Driver PostgreSQL 9.4 JDBC4.1 (build 1205)
2019-06-09 12:47:33.180 +0000 [INFO] (0001:transaction): Using local thread executor with max_threads=4 / output tasks 2 = input tasks 1 * 2
2019-06-09 12:47:33.204 +0000 [INFO] (0001:transaction): {done:  0 / 1, running: 0}
2019-06-09 12:47:33.577 +0000 [INFO] (0017:task-0000): Connecting to jdbc:postgresql://host.docker.internal:5432/jp_test options {ApplicationName=embulk-input-postgresql, user=user, password=***, tcpKeepAlive=true, loginTimeout=300, socketTimeout=1800}
2019-06-09 12:47:33.611 +0000 [INFO] (0017:task-0000): SQL: SET search_path TO "public"
2019-06-09 12:47:33.620 +0000 [INFO] (0017:task-0000): SQL: DECLARE cur NO SCROLL CURSOR FOR SELECT * FROM jp_test WHERE col1 like '%好きなもの%'
2019-06-09 12:47:33.629 +0000 [INFO] (0017:task-0000): SQL: FETCH FORWARD 10000 FROM cur
2019-06-09 12:47:33.637 +0000 [INFO] (0017:task-0000): > 0.00 seconds
2019-06-09 12:47:33.678 +0000 [INFO] (0017:task-0000): SQL: FETCH FORWARD 10000 FROM cur
2019-06-09 12:47:33.689 +0000 [INFO] (0017:task-0000): > 0.01 seconds
好きなもの1
好きなもの2
2019-06-09 12:47:33.722 +0000 [INFO] (0001:transaction): {done:  1 / 1, running: 0}
```

# with digdag modify
add localedef command and set env LANG,LANGUAGE,LC_ALL to enable to use utf-8

```bash
cd with_digdag_mod
docker build . -t jp_test/with_digdag_mod
digdag run jp_test.dig
```

Test was successful
```
2019-06-16 06:13:01.639 +0000 [INFO] (0001:transaction): SQL: SET search_path TO "public"
2019-06-16 06:13:01.654 +0000 [INFO] (0001:transaction): Using JDBC Driver PostgreSQL 9.4 JDBC4.1 (build 1205)
2019-06-16 06:13:01.729 +0000 [INFO] (0001:transaction): Using local thread executor with max_threads=4 / output tasks 2 = input tasks 1 * 2
2019-06-16 06:13:01.735 +0000 [INFO] (0001:transaction): {done:  0 / 1, running: 0}
2019-06-16 06:13:01.887 +0000 [INFO] (0017:task-0000): Connecting to jdbc:postgresql://host.docker.internal:5432/jp_test options {ApplicationName=embulk-input-postgresql, user=user, password=***, tcpKeepAlive=true, loginTimeout=300, socketTimeout=1800}
2019-06-16 06:13:01.919 +0000 [INFO] (0017:task-0000): SQL: SET search_path TO "public"
2019-06-16 06:13:01.929 +0000 [INFO] (0017:task-0000): SQL: DECLARE cur NO SCROLL CURSOR FOR SELECT * FROM jp_test WHERE col1 like '%好きなもの%'
2019-06-16 06:13:01.939 +0000 [INFO] (0017:task-0000): SQL: FETCH FORWARD 10000 FROM cur
2019-06-16 06:13:01.944 +0000 [INFO] (0017:task-0000): > 0.00 seconds
2019-06-16 06:13:01.953 +0000 [INFO] (0017:task-0000): SQL: FETCH FORWARD 10000 FROM cur
2019-06-16 06:13:01.955 +0000 [INFO] (0017:task-0000): > 0.00 seconds
好きなもの1
好きなもの2
2019-06-16 06:13:01.962 +0000 [INFO] (0001:transaction): {done:  1 / 1, running: 0}
2019-06-16 06:13:01.973 +0000 [INFO] (main): Committed.
2019-06-16 06:13:01.974 +0000 [INFO] (main): Next config diff: {"in":{},"out":{}}
```

