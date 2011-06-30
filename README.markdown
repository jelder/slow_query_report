Slow Query Report
=================

This tool makes it a lot easier to stay on top of MySQL performance, especially where use of ORM makes running an EXPLAIN on each query before hand impossible. It has a powerful command line for drilling down into your logs.

    Usage: slow_query_report [OPTIONS]

            --local,   -l      Include local queries (default is to show)
            --remote,  -r      Include remote queries (default is to show)
            --sort     -s      Sort results by this attribute (default is score)
            --squelch, -q      Ignore queries where --sort attribute is less than this (default is 0)
            --top,     -t      Show only the top N queries
            --show-hosts       Include only queries from these hosts
            --hide-hosts       Ignore queries from these hosts
            --show-users       Include only queries from these users
            --hide-users       Ignore queries from these users
            --show-databases   Include only queries on these databases 
            --hide-databases   Ignore queries from these hosts
            --regex   -x       Only consider queries matching this
            --help             Show this help text and exit

    The --local and --remote options can be negated (e.g. --no-local).

    The show and ignore options should be single value or a comma-separated list (e.g. --show-database=mysql,information-schema).

    The --regex option is treated like m/foo/sim.

    Valid sort fields include:
    score Query_seconds_min Query_seconds_median Query_seconds_p95 Query_seconds_max Query_seconds_total Lock_time_min Lock_time_median Lock_time_p95 Lock_time_max Lock_time_total Rows_sent_min Rows_sent_median Rows_sent_p95 Rows_sent_max Rows_sent_total Rows_examined_min Rows_examined_median Rows_examined_p95 Rows_examined_max Rows_examined_total 

Assuming your slow query log gets rotated every day and you keep 7 days of history, the included `mail_slow_query_report` wrapper script can send your whole dev team a list of trouble spots, your five worst queries, each Monday. The email won't go out unless the report is long enough to warrant attention; it won't cry wolf:

`0 7 * * Mon mysql /usr/local/bin/mail_slow_query_report eng@example.com`

For MySQL 5.1 and later:

    [mysqld]
    slow-query-log = true
    slow-query-log-file = /var/log/mysql/mysql-slow.log
    long-query-time = 0.5
