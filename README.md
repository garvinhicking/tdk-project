# tdk-project

Provides a template to be used for "composer create-project" to:

* Create your own project
* Utilize tdk-core and tdk-cli
* Deliver SQL dumps and custom packages
* Have the normal ddev and composer experience
* Enjoy life
* Have some kids

## How to

```
## Initialize example directory and enter it.
mkdir my-tdk-demo && chdir my-tdk-demo

## Initialize a composer project by using a boilerplate
composer create-project garvinhicking/tdk-project

## Install the required modules WITHOUT DEV FIRST (see note)
## !!!VERY IMPORTANT!!!
composer install --no-dev

## Now you can install the actual TYPO3-cms packages as a 
## dev-dependency (you are actually using a dev-setup, so
## in my world, you're good!)
## Using "composer install" at this point would install
## locked versions of the PACKAGIST, so we actually
## run "composer update" at this point to both update
## and install the local packages. Sounds confusing? Yes. It is.
composer update
```



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
