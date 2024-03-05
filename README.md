# PHP Docker Utilities

Helpful utilities/templates for developing, testing, and deploying PHP code in containers.

# PHP Library (CLI) Development Container

* Installs and configures XDebug 3.2.X
* Installs Composer
* Use `PHP_VERSION` build arg to set PHP version

## Example Usage with PhpStorm

Build image and tag as `php-cli-debug`
```
docker build -t php-cli-debug -f cli-develop-with-xdebug/Dockerfile .
```
* In PhpStorm configure a new [remote PHP interpreter **in a Docker container**](https://www.jetbrains.com/help/phpstorm/configuring-remote-interpreters.html)
  * For **Image name** use `php-cli-debug`
  * In [**PHP -> Composer -> Execution**](https://www.jetbrains.com/help/phpstorm/composer-page.html) section
    * Configure as **Remote Interpreter** and select the interpreter you just created

You can now use PhpStorm to run Composer and Run/Debug CLI commands inside a Docker container.