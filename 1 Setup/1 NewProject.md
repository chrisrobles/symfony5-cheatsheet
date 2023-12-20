# [New Project](https://symfony.com/doc/current/setup.html)

Symfony 5.4
1. [Install Symfony CLI](https://symfony.com/download)
   1. download the Symfony CLI
      `curl -sS https://get.symfony.com/cli/installer | bash` 
   2. make command global
      `mv /Users/chris/.symfony/bin/symfony /usr/local/bin/symfony`
   3. test
      `symfony`
2. Install PHP 7.2.5 or higher
   - [Install old version of PHP on MAC](https://github.com/shivammathur/homebrew-php)
     1. `brew tap shivammathur/php`
     2. `brew install shivammathur/php/php@7.4`
     3. `brew link --overwrite --force shivammathur/php/php@7.4`
     4. Restart terminal
     5. `php -v`
   - [Install old version of PHP on linux](https://www.digitalocean.com/community/tutorials/how-to-install-php-7-4-and-set-up-a-local-development-environment-on-ubuntu-20-04)
     1. `sudo add-apt-repository ppa:ondrej/php`
     2. `sudo apt update`
     3. `sudo apt upgrade`
     4. `dpkg -l | grep php` shows `php7.4` package
        - If not
          1. `sudo apt update`
          2. `sudo apt upgrade`
          3. `sudo apt install php7.4 php7.4-cli php7.4-common php7.4-cgi php7.4-cli`
          4. `sudo apt install php7.4-cli`
          5. `dpkg -l | grep php`
     5. `php -v` doesnt show php7.4
        1. `sudo update-alternatives --set php /usr/bin/php7.4`
           - change `/usr/bin/php7.4` to the php7.4 binary
        2. `php -v` should show php7.4
3. Install PHP extensions
   - Ctype, iconv, JSON, PCRE, Session, SimpleXML, and Tokenizer are all installed by default
   - `sudo apt install php7.4-{ctype, iconv, json, pcre, session, simplexml, tokenizer}` doesnt work
4. [Install composer](https://getcomposer.org/download/)
   1. Download
      ```terminal
         php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
         php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
         php composer-setup.php
         php -r "unlink('composer-setup.php');"
         ```
   2. Enable globally
      `mv composer.phar /usr/local/bin/composer`
5. Clone skeleton | Create project
   - Create a new project
      - Use LTS version: `symfony new my_project_directory --version=lts`
      - Specific version: `symfony new my_project_directory --version=5.4`
      - Webapp version: `symfony new --webapp my_project`
   - Set up an existing project
      ```terminal
      $ cd all-projects/
      $ git clone ....
      $ cd my-project/
      $ composer install  # make Composer install the project's dependencies into vendor/
      ```
6. Customize .env file
7. Create database

## PHPStorm Configuration

1. Install Extensions:
    - Symfony
    - PHP Annotations
    - PHP Toolbox
2. Settings->PHP->Composer: Check "Synchronize IDE settings with composer.json"
3. Settings->PHP->Symfony->Enable Plugin for Project
4. Settings->PHP->Symfony->"Web Directory"->Add path to project's public folder
5. Settings->PHP->Symfony->"App Directory"->Add path to project's folder
6. Settings->PHP->Symfony->Twig->Click '+' to add namespace->"Project-Path"->Add path to templates directory

## Run web server

Not working:
1. `lsof -i :8000`
2. `kill -9 <PID>`

### PHP Version

`php -S 127.0.0.1:8000 -t public/`

### EASIER Symfony Version

1. `symfony serve`
   - `--help` see commands options
   - `-d` run as daemon / in background
   - `server:start`
   - `server:status`
   - `server:stop`
2. `$ yarn watch`
