# Fluent Bit Forward Test

This repo contains code to set up a Fluent Bit 2.0.0 client and server that communicate using the Forward protocol. 

To build the plugin and deploy an instance of Fluent Bit with the plugin enabled run the following command:

```sh
docker compose up -d
```

To send a message to the Fluent Bit server you can exec into the running `flb-client` container and use the `nc` utility to send a mssage using TCP:

```sh
nc localhost 5170
msg1
```

You should see the following output in the container logs:

**flb-client**:

```
Fluent Bit v2.0.0
* Copyright (C) 2015-2022 The Fluent Bit Authors
* Fluent Bit is a CNCF sub-project under the umbrella of Fluentd
* https://fluentbit.io

[2022/10/27 10:56:18] [ info] Configuration:
[2022/10/27 10:56:18] [ info]  flush time     | 1.000000 seconds
[2022/10/27 10:56:18] [ info]  grace          | 5 seconds
[2022/10/27 10:56:18] [ info]  daemon         | 0
[2022/10/27 10:56:18] [ info] ___________
[2022/10/27 10:56:18] [ info]  inputs:
[2022/10/27 10:56:18] [ info]      tcp
[2022/10/27 10:56:18] [ info] ___________
[2022/10/27 10:56:18] [ info]  filters:
[2022/10/27 10:56:18] [ info] ___________
[2022/10/27 10:56:18] [ info]  outputs:
[2022/10/27 10:56:18] [ info]      forward.0
[2022/10/27 10:56:18] [ info]      stdout.1
[2022/10/27 10:56:18] [ info] ___________
[2022/10/27 10:56:18] [ info]  collectors:
[2022/10/27 10:56:18] [ info] [fluent bit] version=2.0.0, commit=ad86380097, pid=1
[2022/10/27 10:56:18] [debug] [engine] coroutine stack size: 24576 bytes (24.0K)
[2022/10/27 10:56:18] [ info] [storage] ver=1.3.0, type=memory, sync=normal, checksum=off, max_chunks_up=128
[2022/10/27 10:56:18] [ info] [cmetrics] version=0.5.3
[2022/10/27 10:56:18] [ info] [ctraces ] version=0.2.5
[2022/10/27 10:56:18] [ info] [input:tcp:tcp.0] initializing
[2022/10/27 10:56:18] [ info] [input:tcp:tcp.0] storage_strategy='memory' (memory only)
[2022/10/27 10:56:18] [debug] [tcp:tcp.0] created event channels: read=21 write=22
[2022/10/27 10:56:18] [debug] [downstream] listening on 0.0.0.0:5170
[2022/10/27 10:56:18] [debug] [forward:forward.0] created event channels: read=24 write=25
[2022/10/27 10:56:18] [debug] [stdout:stdout.1] created event channels: read=36 write=37
[2022/10/27 10:56:18] [debug] [router] match rule tcp.0:forward.0
[2022/10/27 10:56:18] [ info] [output:forward:forward.0] worker #0 started
[2022/10/27 10:56:18] [debug] [router] match rule tcp.0:stdout.1
[2022/10/27 10:56:18] [ info] [sp] stream processor started
[2022/10/27 10:56:18] [ info] [output:forward:forward.0] worker #1 started
[2022/10/27 10:56:18] [ info] [output:stdout:stdout.1] worker #0 started
[2022/10/27 10:56:42] [debug] [input chunk] update output instances with new chunk size diff=21
[2022/10/27 10:56:43] [debug] [task] created task=0x7fef1ee3da10 id=0 OK
[2022/10/27 10:56:43] [debug] [output:forward:forward.0] task_id=0 assigned to thread #0
[2022/10/27 10:56:43] [debug] [output:stdout:stdout.1] task_id=0 assigned to thread #0
[2022/10/27 10:56:43] [debug] [output:forward:forward.0] request 21 bytes to flush
[2022/10/27 10:56:43] [debug] [output:forward:forward.0] send options records=1 chunk='3c9271170e085ad57edf963fd3085d09'
[2022/10/27 10:56:43] [debug] [out flush] cb_destroy coro_id=0
[0] tcp.0: [1666868202.807231270, {"log"=>"msg1"}]
[2022/10/27 10:56:43] [error] [output:forward:forward.0] ack: ack len does not match ack(32)(3c9271170e085ad57edf963fd3085d09) chunk(0)()
[2022/10/27 10:56:43] [debug] [out flush] cb_destroy coro_id=0
[2022/10/27 10:56:43] [debug] [retry] new retry created for task_id=0 attempts=1
[2022/10/27 10:56:43] [ warn] [engine] failed to flush chunk '1-1666868202.807254959.flb', retry in 11 seconds: task_id=0, input=tcp.0 > output=forward.0 (out_id=0)
```

**flb-server:**
```
Fluent Bit v2.0.0
* Copyright (C) 2015-2022 The Fluent Bit Authors
* Fluent Bit is a CNCF sub-project under the umbrella of Fluentd
* https://fluentbit.io

[2022/10/27 10:56:18] [ info] Configuration:
[2022/10/27 10:56:18] [ info]  flush time     | 1.000000 seconds
[2022/10/27 10:56:18] [ info]  grace          | 5 seconds
[2022/10/27 10:56:18] [ info]  daemon         | 0
[2022/10/27 10:56:18] [ info] ___________
[2022/10/27 10:56:18] [ info]  inputs:
[2022/10/27 10:56:18] [ info]      forward
[2022/10/27 10:56:18] [ info] ___________
[2022/10/27 10:56:18] [ info]  filters:
[2022/10/27 10:56:18] [ info] ___________
[2022/10/27 10:56:18] [ info]  outputs:
[2022/10/27 10:56:18] [ info]      stdout.0
[2022/10/27 10:56:18] [ info] ___________
[2022/10/27 10:56:18] [ info]  collectors:
[2022/10/27 10:56:18] [ info] [fluent bit] version=2.0.0, commit=ad86380097, pid=1
[2022/10/27 10:56:18] [debug] [engine] coroutine stack size: 24576 bytes (24.0K)
[2022/10/27 10:56:18] [ info] [storage] ver=1.3.0, type=memory, sync=normal, checksum=off, max_chunks_up=128
[2022/10/27 10:56:18] [ info] [cmetrics] version=0.5.3
[2022/10/27 10:56:18] [ info] [ctraces ] version=0.2.5
[2022/10/27 10:56:18] [ info] [input:forward:forward.0] initializing
[2022/10/27 10:56:18] [ info] [input:forward:forward.0] storage_strategy='memory' (memory only)
[2022/10/27 10:56:18] [debug] [forward:forward.0] created event channels: read=21 write=22
[2022/10/27 10:56:18] [debug] [in_fw] Listen='0.0.0.0' TCP_Port=24224
[2022/10/27 10:56:18] [debug] [downstream] listening on 0.0.0.0:24224
[2022/10/27 10:56:18] [ info] [input:forward:forward.0] listening on 0.0.0.0:24224
[2022/10/27 10:56:18] [debug] [stdout:stdout.0] created event channels: read=24 write=25
[2022/10/27 10:56:18] [debug] [router] match rule forward.0:stdout.0
[2022/10/27 10:56:18] [ info] [sp] stream processor started
[2022/10/27 10:56:18] [ info] [output:stdout:stdout.0] worker #0 started
[2022/10/27 10:56:43] [debug] [input chunk] update output instances with new chunk size diff=21
[2022/10/27 10:56:44] [debug] [task] created task=0x7fa91123c770 id=0 OK
[2022/10/27 10:56:44] [debug] [output:stdout:stdout.0] task_id=0 assigned to thread #0
[2022/10/27 10:56:44] [debug] [out flush] cb_destroy coro_id=0
[0] tcp.0: [1666868202.807231270, {"log"=>"msg1"}]
[2022/10/27 10:56:44] [debug] [task] destroy task=0x7fa91123c770 (task_id=0)
```
