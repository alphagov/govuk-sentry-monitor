require 'sentry-api'
require 'statsd'

PROJECTS_TO_IGNORE = %w[test-ecs]

SentryApi.configure do |config|
  config.endpoint = 'https://sentry.io/api/0'
  config.auth_token = ENV.fetch('SENTRY_AUTH_TOKEN')
  config.default_org_slug = 'govuk'
end

task :run do
  statsd = Statsd.new

  since = Time.now.to_i - (60 * 60)

  SentryApi.organization_projects.each do |project|
    next if PROJECTS_TO_IGNORE.include? project.slug

    stats = SentryApi.project_stats(project.slug, since: since)

    error_count = stats.reduce(0) do |sum, (_timestamp, count)|
      sum + count
    end

    statsd.gauge "sentry-error-count-last-hour.#{project.name}", error_count

    puts "#{project.slug}: #{error_count}"
  end
end
