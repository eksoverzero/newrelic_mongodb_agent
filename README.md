# New Relic+MongoDB Extension

**Repurposed from https://github.com/MongoHQ/newrelic-mongodb-agent**

A plugin to gather metrics from your MongoDB deployment and send the
metrics to the New Relic Platform.

## Requirements

While this project is gaining steam, you will need a base knowledge of
Ruby and Ruby Gems.  If you have this knowledge, and feel that you
breezed through the installation, please consider writing a tutorial for
others.  If you are lacking knowledge on the Ruby and Ruby gems
configuration, please post questions to the Issues section of the
project, and we will help as quickly as possible.

## Metrics

Everyone loves metrics.  Everyone loves metrics beside their application
metrics.  The New Relic platform is a hit.

MongoDB will monitor the following metrics:

* Operations / Second
* RAM Usage
* Disk Size
* Lock %
* Page Faults

This extension will work for single servers, replica sets, and sharding.

## Using with Replica Sets and Sharding

To use with Replica Sets and Sharding, you will need to run one agent
per host being monitored.  In the future, we are planning to do
inspection and automated monitoring for complete environments.

## Instructions for running the MongoDB plugin agent

1. run `bundle install` to install required gems
2. Copy `config/newrelic_plugin.yml.example` to `config/newrelic_plugin.yml`
3. Edit `config/newrelic_plugin.yml` and replace "YOUR_LICENSE_KEY_HERE" with your New Relic license key
4. Edit the `config/newrelic_plugin.yml` file and add MongoDB connection and database settings
5. Running the plugin

In order to check your configuration, you can launch the plugin
in foreground mode, with all output going to stdout:

  ./newrelic_mongodb_agent

Carefully check plugin's output for any possible error messages.
In case of success, collected data should appear in the New Relic
user interface shortly after starting.

Plugin can also be started as a daemon using the following command:

  ./newrelic_mongodb_agent.daemon start

In this case you can check its status by running

  ./newrelic_mongodb_agent.daemon status

and stop it with

  ./newrelic_mongodb_agent.daemon stop

### Monit example

```
check process newrelic_mongodb_agent
  with pidfile /home/ubuntu/newrelic_mongodb_agent/newrelic_mongodb_agent.pid
  start program = "/bin/su - ubuntu -c '/home/ubuntu/newrelic_mongodb_agent/newrelic_mongodb_agent.daemon start'" with timeout 90 seconds
  stop program = "/bin/su - ubuntu -c '/home/ubuntu/newrelic_mongodb_agent/newrelic_mongodb_agent.daemon stop'" with timeout 90 seconds
  if totalmem is greater than 250 MB for 2 cycles then restart
  group newrelic_agent
```

### Supervisord example

```
[program:newrelic_mongodb]
command = bash -c ./newrelic_mongodb_agent
directory = /opt/newrelic_mongodb_agent
autostart = true
autorestart = true
startretries = 10
user = root
startsecs = 10
redirect_stderr = true
stdout_logfile_maxbytes = 50MB
stopwaitsecs = 10
```
