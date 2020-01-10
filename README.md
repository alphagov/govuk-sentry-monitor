# GOV.UK Sentry Monitor

Script to report the number of errors in Sentry to Graphite. The result is displayed on the [Sentry dashboard][sentry-d] and on the [2nd line dashboard][2nd].


## How it works

- The [govuk-sentry-monitor jenkins job][jj] runs the [script in the Rakefile][rr] every 15 minutes
- The script uses the Sentry API to[ fetch the stats per project][stats] for the last hour
- The script then send the total count per app for the last hour to the Statsd process as a  ["gauge"][gauge]
- Statsd [sends the metric over to Graphite][graphite], which means we can [use it in Grafana][sentry-d]

[sentry-d]: https://grafana.publishing.service.gov.uk/dashboard/file/sentry_errors_across_all_environments.json
[2nd]: https://grafana.publishing.service.gov.uk/dashboard/file/2ndline_health.json
[jj]: https://deploy.publishing.service.gov.uk/job/Check_Sentry_Errors/
[rr]: /Rakefile
[stats]: https://docs.sentry.io/api/projects/get-project-stats/
[gauge]: http://statsd.readthedocs.io/en/v0.5.0/types.html#gauges
[graphite]: https://graphite.publishing.service.gov.uk/
