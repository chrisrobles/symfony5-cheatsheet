
# bin/console

A powerful debugging tool (not exclusive to symfony)

`$ php bin/console`  `#shows list of commands`

## Install bash completion

`$ php bin/console completion bash | sudo tee /etc/bash_completion.d/console-events-terminate`

## about: | Learn about a symfony project

`php bin/console about`

## server: | Run a local server

1) `$ cd my-project/`
2) `$ symfony server:start`
3) will open at <http://localhost:8000/>
4) `ctrl+c` from browser to stop server
- *Tip*: works with any php application not just symfony projects

## debug:

### List Routes

`php bin/console debug:router`

### List Services

`php bin/console debug:autowiring`

#### Find Specific Service

Find a log service

`php bin/console debug:autowiring log`

## check:

- `$ symfony check:security` | Security of bundles
  - Will show compromised dependencies that need to be updated or replaced
  - Check done locally (compared to fetched [PHP security advisories database](https://github.com/FriendsOfPHP/security-advisories))
  - Returns a non-zero exit code if any of your dependencies are vulnerable

## list debug

- see the list of all commands available