# Command

- Must extend CommandBase and return `$this->success(?$endpoint)` if they complete.

## Workers

Executes long running stuff

1) User requests some long-running thing via the GUI (data export)
2) Controller creates task in queue
3) Worker picks up item from queue

- Work well with queues
  - Done with purpose-built queue or "jobs" table in MySQL
- Consider
  - chance of failure
  - how many workers processing jobs in queue
    