# Filesystem Optimizations

## Which filesystem is best?

The filesystem is not the most important aspect of MariaDB performance. Far more important are available RAM, drive speed, the system variable settings (see [Hardware Optimization](/replication/optimization-and-tuning/hardware-optimization/) and [System Variables](/replication/optimization-and-tuning/system-variables/)).

Optimizing the filesystem can however in some cases make a noticeable difference. Currently, the best Linux filesystems are generally regarded as ext4, XFS and Btrfs. They are all included in the mainline Linux kernel, and are widely supported and available on most Linux distributions. Red Hat though regards Brtfs as a technology preview, not yet ready for production systems.

The following theoretical file size and filesystem size limits apply to the three filesystems:

<table><tbody><tr><td></td><th>ext4</th><th>XFS</th><th>Brtfs</th></tr>
<tr><td>Max file size</td><td>16TB</td><td>8EB</td><td>16EB</td></tr>
<tr><td>Max filesystem size</td><td>1 EB</td><td>8EB</td><td>16EB</td></tr>
</tbody></table>

Each has unique characteristics that are worth understanding to get the most from.

## Disabling access time

It's unlikely you'll need to record file access time on a database server, and mounting your filesystem with this disabled can give an easy improvement in performance. To do so, use the `noatime` option.

If you want to keep access time for [log files](/kb/en/log-files/) or other system files, these can be stored on a separate drive.