# Tutorial Installation of Laminas MVC and digesting it

What to expect and what you'll learn in my tutorial for Laminas MVC.
 

1. Creation of your own Module.
2. Extending an existing module.
3. Usage of Doctrine ORM.
4. Usage of Command Line Interface(CLI)
5. User/Member creation via an existing module.
6. Assigning roles to an existing member/user.
7. Sending Email when registering a member/user.
8. Crud for creating a post.
9. Listening to an event and doing some of your own stuff.

 
So, first, we need to install Laminas MVC and we'll use the minimum installation to start our tutorial. I'm assuming you the viewers are Linux users. If you guys don't use Linux for development. I highly recommend it.

```
sudo apt-get install php8.1 php8.1-mbstring php8.1-intl php8.1-xml \ 
php8.1-bcmath php8.1-common php8.1-curl php8.1-mysql php8.1-gd
 
composer create-project -sdev laminas/laminas-mvc-skeleton ./LrphptLaminasTutorial
cd LrphptLaminasTutorial

composer install
// Press "Y" for minimal installation. Because we'll be using Doctrine, not Laminas-DB
 
composer require "doctrine/doctrine-orm-module:^5"
composer require "doctrine/migrations:^3.6.0"
composer require "gedmo/doctrine-extensions:^3.12.0"
composer require "laminas/laminas-session:^2.16"
composer require "laminas/laminas-permissions-rbac:^3.0"
composer require "lm-commons/lmc-user-doctrine-orm:^3.0"
composer require "lm-commons/lmc-rbac-mvc:^v1.2"
composer require "san/san-session-toolbar:^4.0"
composer require "laminas/laminas-cli:^1.8"
```

If everything went smoothly for you people till here then you should be able to run the built-in php server command to see the home page of laminas, which is looks like below:

So, let us start by digesting what we've just installed. So, as you've seen we went with the minimal installation and installed the modules which we'll be working with and how to modify them for our use. Which will help us how to modify existing modules present on GitHub or any paid platform. But first, we need to look at the configuration provided by the Lamnias MVC and what does it contain. **So let us see what the application config file in the config folder contains.**
 
## Use Laminas Loader:
  This configuration is set to false because from Laminas MVC 3.0 version the modules which anyone creates will be defined in the composer.json file. Meaning the mapping of the classes with respect to modules will be defined there under the Psr-4. It may help for Laminas Version 2.0 when set to true.

## Config Global Paths:
  This configuration key tells us that we can have multiple files by the names global.php and local.php. Meaning that we can have database.global.php, databse.local.php and so on and so forth. This is just an example. So, what is the use of global.php and local.php file? The reason behind this is that everyone in the development team has some base configuration to work with without sharing any sensitive information about the project. For example, for a database, everyone should be using the same base configuration like database name, and character set to use without sharing sensitive information like username and password.
 
## Config Cache Enabled:
  This configuration is for production usually. Because a configuration cache file is only generated in production mode and I don't know how to generate a cache file in development mode.
 
## Config Cache Key:
  This configuration lets you change or let it be as it is for the creation of the cache key which will be used to access the cache if you use any cache. The cache is generated in production mode to speed up the configuration loading time.
 
## Cache Dir:
  This key tells us where any cache file or other data will be stored by any module. Doctrine uses this directory to store some configuration. If anyone wants to change it they can change it.
 
Now let us move our attention to the configuration provided in the Application Modules folder and see what it contains.
 
When we look at it there are four essential keys which are specific to the Laminas MVC framework therefore we can't use these names for our purpose. Meaning we can define any of our module-specific information in them. For example, a router will only contain laminas mvc router information rather than any google maps router or network router information. So, lets us take a look at the Laminas MVC reserved keys which are used by Laminas MVC and the first six keys are commonly used when developing a module.

1. Router
2. Controllers
3. Service Manager
4. View Helper
5. View Manager
6. Factories
7. Invokables
8. Initializer
9. Listeners
10. Validators
11. Filters
12. Controller Plugins

In the next tutorial, we'll be going learn to use some commands of Doctrine and we'll be setting up our user module to start the registration process and dieset the LmcUser module.
