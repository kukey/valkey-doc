Instruct Valkey to start an [Append Only File][tpaof] rewrite process.
The rewrite will create a small optimized version of the current Append Only
File.

[tpaof]: ../topics/persistence.md#append-only-file

If `BGREWRITEAOF` fails, no data gets lost as the old AOF will be untouched.

The rewrite will be only triggered by Valkey if there is not already a background
process doing persistence.

Specifically:

* If a Valkey child is creating a snapshot on disk, the AOF rewrite is _scheduled_ but not started until the saving child producing the RDB file terminates. In this case the `BGREWRITEAOF` will still return a positive status reply, but with an appropriate message.  You can check if an AOF rewrite is scheduled looking at the `INFO` command.
* If an AOF rewrite is already in progress the command returns an error and no
  AOF rewrite will be scheduled for a later time.
* If the AOF rewrite could start, but the attempt at starting it fails (for instance because of an error in creating the child process), an error is returned to the caller.

The AOF rewrite is automatically triggered by Valkey, however the
`BGREWRITEAOF` command can be used to trigger a rewrite at any time.

Please refer to the [persistence documentation][tp] for detailed information.

[tp]: ../topics/persistence.md

