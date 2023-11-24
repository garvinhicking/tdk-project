# tdk-project

Provides a template to be used for "composer create-project" to:

* Create your own project
* Utilize tdk-core and tdk-cli
* Deliver SQL dumps and custom packages
* Have the normal ddev and composer experience
* Enjoy life
* Have some kids

## How to

### Create from scratch

```
## Initialize a composer project by using a boilerplate
composer create-project --stability dev garvinhicking/tdk-project my-tdk-demo 

## Change to the new directory
chdir my-tdk-demo

## Now you can install the actual TYPO3-cms packages as a 
## dev-dependency (you are actually using a dev-setup, so
## in my world, you're good!)
## Using "composer install" at this point would install
## locked versions of the PACKAGIST, so we actually
## run "composer update" at this point to both update
## and install the local packages. Sounds confusing? Yes. It is.
composer update
```

### Create from existing repository

The project created via the method above can be committed to your own repository.
You can then clone your directory and set it up with the TYPO3-core via:

```
## Clone your repository (can be anything, must not be GitHub)
git clone git@github.com:your-vendor/your-repo.git my-tdk-demo

## Change to directory
cd my-tdk-demo

## Install the required modules WITHOUT DEV FIRST (see note)
## !!!VERY IMPORTANT!!!
composer install --no-dev

## That will have then checked out your sources. Now you should
## be able to update composer and install the dev-dependencies
## properly.
composer update
```

### Follow-up: DDEV

This project also provides a default DDEV configuration.

You can use `ddev start` to start it.

There is basic configuration provided with the tdk-project dummy,
but you'll need to initialize the database. This is done
easiest via `ddev ssh` and then:

```
  TYPO3_DB_DRIVER=mysqli \
  TYPO3_DB_USERNAME=db \
  TYPO3_DB_PORT=3306 \
  TYPO3_DB_HOST=db \
  TYPO3_DB_DBNAME=db \
  TYPO3_DB_PASSWORD=db \
  TYPO3_SETUP_ADMIN_PASSWORD="SuperDuper0815." \
  TYPO3_SETUP_ADMIN_EMAIL=admin@example.com \
  TYPO3_SETUP_ADMIN_USERNAME=admin \
  TYPO3_SETUP_CREATE_SITE="https://tdk-project.ddev.site/" \
  TYPO3_PROJECT_NAME="TYPO3 Development Kit" \
  TYPO3_SERVER_TYPE="apache" \
  ./vendor/bin/typo3 setup --force
```

If you spin-off your own project, you should:

* Edit `.ddev/config.yaml` and configure your own hostname
* Provide your own `config/sites/tdk-project/` Site configuration
* Provide your own `config/system/*.php` Core configuration

## Notes

Currently a nasty workaround is employed that tries to solve a
specific problem.

We can only list and properly install `typo3/cms-*` packages in `composer.json`,
once the `vendor/bin/tdk-cli clone` command has succeeded. Because without
the local repository, these packages cannot be installed from the local GIT repository.

And the local git repository cannot be created before the packages are listed,
because people may check out a project without yet having the GIT working directory.

We need to put `composer.json` inside this repository for people to be able to
spin-off a custom project and make it run.

Now the workaround is that we use "require-dev" on the actual TYPO3 packages, and 
first start a "composer install" by not requiring the packages (where we would not
be able to find the local packages)

Surely there's a better way for it. I just haven't found that yet. Help! :-)

## TODO

* .ddev config
* CLI asking which extensions to be loaded
* add githooks
* add git commit template
* ...?
