# Tutorial 2 - To use LmcUser in Laminas MVC a user creation module with Doctrine

## Let us pass our first PHPUnit test in Laminas MVC.

As we all know that a user/member module is essential in any application. Because there are many features related to a member like subscriptions, posting, purchases, trading, betting etc. So, let us start using LmcUser and try to understand what it offers. But before that let us pass our first test in Laminas MVC. To do that we need to run the below command and see that it fails. Then we'll fix the issue and run the test again to make it a success.

```
composer test
```

You'll see that it fails and gives an error. Now, we need to fix it right. So, open the class ApplicationTest in the application module test folder and change the line which reads like the below with the provided command.

```
    public function testIndexActionViewModelTemplateRenderedWithinLayout(): void
    {
        $this->dispatch('/', 'GET');
        $this->assertQuery('.container .rounded-3');
    }
```

## Setting up Doctrine and Doctrine migrations in Laminas MVC.

So, what was the cause of this issue? If you've guessed it that is great if not then let me share it with you. The test which we've just fixed was trying to check if the dom contains the class container and within that container, a class rounded 3. It failed before because this test is old and from the days of version and with bootstrap version 3. That is why when it tried to check the container class containing the jumbotron class it failed. Because the HTML has changed in Laminas Version 3. There you've it.  For me, it was a great joy when I first fixed it and I don't know about you guys. Now let us proceed in the direction of LmcUserDoctrine. Let's go.

The first step we need to take is to define our database configuration for doctrine and doctrine migrations. So, let's get started. We need to first create a file in **"config/autoload/doctrine.global.php"** and fill it up with the below code.

```
<?php
 
declare(strict_types=1);
 
namespace Application;
return [
    'doctrine' => [
        'connection' => [
            'orm_default' => [
                'driverClass' => \Doctrine\DBAL\Driver\PDO\MySQL\Driver::class,
                'params' => [
                    'host'     => 'localhost',
                    'charset'  => 'utf8mb4',
                    'collate'  => 'utf8mb4_unicode_ci',
                ],
            ],
        ],
        'eventmanager' => [
            'orm_default' => [
                'subscribers' => [
                    // pick any listeners you need
                    \Gedmo\Tree\TreeListener::class,
                    \Gedmo\Timestampable\TimestampableListener::class,
                    \Gedmo\Sluggable\SluggableListener::class,
                    \Gedmo\Loggable\LoggableListener::class,
                ],
            ],
        ],
        'driver' => [
            // defines an annotation driver with two paths, and names it `lrphpt_annotation_driver`
            'lrphpt_annotation_driver' => [
                'class' => \Doctrine\ORM\Mapping\Driver\AnnotationDriver::class,
                'cache' => 'array',
                'paths' => [
                    realpath(__DIR__ . '/../../module/Application/src/Entity'),
                    realpath(__DIR__ . '/../../vendor/gedmo/doctrine-extensions/src/Loggable/Entity'),
                ],
            ],
            // default metadata driver, aggregates all other drivers into a single one.
            // Override `orm_default` only if you know what you're doing
            'orm_default' => [
                'drivers' => [
                    // register `lrphpt_annotation_driver` for any entity under namespace `My\Namespace`
                    'Application\\Entity' => 'lrphpt_annotation_driver',
                    'Gedmo\\Loggable\\Entity' => 'lrphpt_annotation_driver',
                ],
            ],
        ],
        'migrations_configuration' => [
            'orm_default' => [
                'table_storage' => [
                    'table_name' => 'migrations_executed',
                    'version_column_name' => 'version',
                    'version_column_length' => 255,
                    'executed_at_column_name' => 'executed_at',
                    'execution_time_column_name' => 'execution_time',
                ],
                'migrations_paths' => ['Lrphpt\Migrations' => realpath(__DIR__ . '/../../scripts/orm/migrations')],
                'all_or_nothing' => true,
                'check_database_platform' => true,
            ],
        ],
    ],
];
```
**Now update local.php with the database password and username.** You can create a seprate file like databae.local.php as well.
```
return [
// your other config
    'doctrine' => [
        'connection' => [
            // default connection name
            'orm_default' => [
                'driverClass' => \Doctrine\DBAL\Driver\PDO\MySQL\Driver::class,
                'params' => [
                    'host'     => 'localhost',
                    'port'     => '3306',
                    'user'     => 'root',
                    'password' => 'password',
                    'dbname'   => 'LrphptLaminasMVCTutorial',
                ],
            ],
        ],
    ],
];

```

**Now we need to copy LmcUser global php from the LmcUser vendor directory to "config/autoload/lmc_user.global.php" and edit the line shown below:**

```
return [
    // only change the below line and let everything as it is.
    'user_entity_class' => \Application\Entity\User::class,
];
```

## Let us create User Entity.

Now let us **create a User entity class in "module/Application/src/Entity/User.php",** so let us begin. The reason to create our own entity is because we're using annotations for doctrine and we need to define our entity with Doctrine Annotations.

```
<?php
declare(strict_types=1);
 
namespace Application\Entity;
 
use Doctrine\ORM\Mapping as ORM;
use Gedmo\Mapping\Annotation as Gedmo; // this will be like an alias for Gedmo extensions annotations
use LmcUser\Entity\UserInterface;
 
/**
 * User
 *
 * @ORM\Table(name="user",
 * uniqueConstraints={@ORM\UniqueConstraint(name="email", columns={"email"}),
 * @ORM\Index(name="user_email_idx_idx", columns={"email"})})
 * @ORM\Entity
 */
class User implements UserInterface
{
    /**
     * @var int
     *
     * @ORM\Column(name="id", type="bigint", nullable=false)
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="IDENTITY")
     */
    private $id;
 
    /**
     * @var string
     *
     * @ORM\Column(name="email", type="string", length=150, nullable=false)
     */
    private $email;
 
    /**
     * @var string
     *
     * @ORM\Column(name="password", type="string", length=150, nullable=false)
     */
    private $password;
 
    /**
     * @var string
     *
     * @ORM\Column(name="username", type="string", length=150, nullable=true)
     */
    private $username;
 
    /**
     * @var string|null
     *
     * @ORM\Column(name="displayname", type="string", length=250, nullable=true)
     */
    private $displayname;
 
    /**
     * @var bool|null
     *
     * @ORM\Column(name="state", type="boolean", nullable=true, options={"default"="1"})
     */
    private $state = true;
 
    /**
     * @var \DateTime
     *
     * @Gedmo\Timestampable(on="create")
     * @ORM\Column(name="created_at", type="datetime", nullable=false)
     */
    private $createdAt;
 
    /**
     * @var \DateTime
     *
     * @Gedmo\Timestampable(on="update")
     * @ORM\Column(name="updated_at", type="datetime", nullable=false)
     */
    private $updatedAt;
    
    public function __construct()
    {
    }
    
    public function getId(){
        return $this->id;
    }
 
    public function setId($id){
        $this->id = $id;
        return $this;
    }
 
    public function getPassword(){
        return $this->password;
    }
 
    public function setPassword($password){
        $this->password = $password;
        return $this;
    }
 
    public function getUsername(){
        return $this->username;
    }
 
    public function setUsername( $username){
        $this->username = $username;
        return $this;
    }
 
    public function getDisplayName(){
        return $this->displayname;
    }
 
    public function setDisplayName($displayname){
        $this->displayname = $displayname;
        return $this;
    }
 
    public function getState(){
        return $this->state;
    }
 
    public function setState($state){
        $this->state = $state;
        return $this;
    }
 
    public function getEmail(){
        return $this->email;
    }
 
    public function setEmail($email){
        $this->email = $email;
        return $this;
    }
}
```

## Laminas MVC Modules needed to run LmcUserDoctrine.

Some of you might be wondering why I created a User entity when it is present in the LmcUser module. Also, why I didn't extend it? The reason being we're using Doctrine Annotations and the entity User in LmcUser module doesn't have annotations in it. Therefore doctrine doesn't understand what to do with the User entity. That is the reason I've chosen to use the UserInterface to implement the entity. Which is for doctrine the correct way I've tested it many times and it works. I hope I've cleared this point for you people. Now it is time to create a user table or entity via the Doctrine migration command. But before that, we need to make sure our module.config.php file looks like the below:

```
return [
    'Laminas\Mvc\Plugin\Prg',
    'Laminas\Mvc\Plugin\FlashMessenger',
    'Laminas\I18n',
    'Laminas\Session',
    'Laminas\Cache',
    'Laminas\Form',
    'Laminas\Hydrator',
    'Laminas\InputFilter',
    'Laminas\Filter',
    'Laminas\Paginator',
    'Laminas\Router',
    'Laminas\Validator',
    'DoctrineModule',
    'DoctrineORMModule',
    'LmcUser',
    'LmcUserDoctrineORM',
    'Application',
];
 
// We need to do a final configuration before we can proceed.
// Open module/Application/config/module.config.php file and add the below configuration so that everything works seamlessly.
 
use DoctrineORMModule\Service\DoctrineObjectHydratorFactory;
 
return [
    // your other configuration
    'service_manager' => [
        'alias' => [
            'lmcuser_base_hydrator' => DoctrineObjectHydratorFactory::class,
        ],
        'factories' => [
            'lmcuser_user_hydrator' => DoctrineObjectHydratorFactory::class,
        ],
    ],
];
```

**Now we're set to run the below commands.**
```
./vendor/bin/doctrine-module migrations:diff
 
// It gives an error aaarrrgghh!!! Relax! You're not dealing with an AI here. 
// Try to do something yourself. Type the command below.
mkdir -p myscripts/myorm/mymigrations
 
// to let apache write in the data directory.
chmod -R $USER:www-data ./data
 
./vendor/bin/doctrine-module migrations:diff
 
./vendor/bin/doctrine-module migrations:migrate
 
//Run the web server via php by entering the below command if not running already.
 
composer serve
```

After this, you should have access to the following URLs.
1. localhost/user/login
2. localhost/user/register
3. localhost/user/change-password
4. localhost/user/change-email

But when you'll register a member/user you would want to send an email for verification. But you'll see that no email goes out and you'll start crying. So, don't be sad. I'll teach you how to send email via LmcUser.
