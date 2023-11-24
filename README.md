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

## Just do it

You should now have a working TYPO3 environment with backend and frontend.

Your directory `typo3-core` is a distinct GIT working directory.

You can go to that directory and perform usual `git` operations, like
pulling, cherry-picking.

You could theoretically also change branches, but you will have to adjust
your composer.json then to reflect version numbers. This repository is
aimed at `main` development (currently: TYPO3 v13).

After using `git pull` to update to the most recent TYPO3 changes, you
should maybe also run `composer update` in your root directory. This
allows you to be sure that dependencies can be resolved.

Within the `typo3-core` directory you can ALSO run a `composer install`
so that all development packages are installed within that tree. This will
allow you to run usual developer tasks, like the `Build/Scripts/runTests.sh`
script together with `phpstan` etc.

When you contribute to the TYPO3-core like that, you can use your working
directory within this project. Just remember that you need to operate GIT
within that `typo3-core` directory. If you operate GIT outside that directory,
you are doing changes to the `tdk-project`-specific directory. This one never
holds TYPO3-specific core information, it's only a wrapper so that you can
utilize `TYPO3` in composer mode.

The largest benefit is that you can put custom extensions into the `/packages/`
subdirectory, and require those with `composer` (in your project root, not
the `typo3-core` subdirectory). These extensions will then be available 
in your TYPO3-Installation.

And it allows you to store these extension in the main project repository,
so other people can easily check out aspects of the `TYPO3 Core` with your
extension provided.

You can also add custom `SQL dump` files to this repository and allow easy
(re)import via `ddev import-db` (and `ddev export-db`).

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

* CLI asking which extensions to be loaded
* add githooks
* add git commit template
* ...?
