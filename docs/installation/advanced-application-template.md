Advanced Application Template
=============================

This page is dedicated for those who prefer to use more or less default
[Advanced Application Template](https://github.com/yiisoft/yii2-app-advanced) provided by the Yii core team. Please
check [their readme](https://github.com/yiisoft/yii2-app-advanced#readme) file on why and when you should use it,
for now we're going to explain on how Yii2 User Module extension could be used for the case if you prefer it.

> We highly recommend you to use [Sidekit Application Template](http://www.2amigos.us/) which has sensible default
packages, and everything you need to start project in no time with batteries included! ;-)
>
> [Check this manual page](sidekit-application-template.md) if you decided to use it.

If you want to use the default [Basic Application Template](https://github.com/yiisoft/yii2-app-basic) with our
extension, then check [Basic Application Template documentation page](basic-application-template.md).

Step 1 - Install Advanced Application template
----------------------------------------------

Please check [this document](https://github.com/yiisoft/yii2-app-advanced/blob/master/docs/guide/start-installation.md)
on how you can install Advanced Application Template. We're not going to cover this process here.

Step 2 - Configure your application
-----------------------------------

For now we will consider you need to setup user module for the backend tier of your project. User module needs to set
and instantiate various settings, services, and parts to be used in application. That's why you have to include
bootstrap class in your application bootstrapping workflow by adding the following lines in your
`%PROJECT_DIR%/backend/config/main.php`:

```php
return [
    // ... existing settings of the application
    'modules' => [
        'user' => [
            'class' => Da\User\Module::class,
        ],
    ],
    'components' => [
        // ... configuration of the existing application components
    ],
];
```

Our extension implemented as some kind of substructure containing its own MVC entities, so another thing you have
to register it as an application module by adding following lines to the same configuration script file:

```php
return [
    // ...
    'modules' => [
        'user' => [
            'class' => Da\User\Module::class,
        ],
    ],
    'components' => [
        // ...
    ],
];
```

That's all from the part of the web application.

Step 3 - Apply database schema migrations
-----------------------------------------

This is obvious extension like our which deals with users, roles, permissions, etc. have to use some database.
Our migrations are namespaced and available in `Da\User\Migration` namespace.

Before starting to work with database, please ensure you have deleted `m130524_201442_init.php` migration file
which comes from the default installation of the Advanced Application Template. It's located at
`%PROJECT_DIR%/console/migrations/m130524_201442_init.php` path.

There are two ways to apply migrations of this extension, the first one:

```php
./yii migrate --migrationPath="@Da/User/Migration"
./yii migrate
```

First command applies migration set of the user module, and the second one is for application migrations.

> Note, you cannot mix two ways: choose one of them, and stick with it.

The second way is more comfortable, and you don't have to remember to launch first command every time you obtain
new version of our extension. First of all add the following lines to the file
`%PROJECT_DIR%/console/config/main.php`:

```php
return [
    // ...
    'controllerMap' => [
        'migrate' => [
            'class' => \yii\console\controllers\MigrateController::class,
            'migrationNamespaces' => [
                'Da\User\Migration',
            ],
        ],
    ],
    // ...
];
```

This basically instructs your application to always try to use migrations from the given namespace. Which again
is very convenient way to track new migration classes coming from this and possibly other extensions and sources.

> Namespaced migrations were introduced in Yii 2.0.10, so before using them consider updating your framework
> installation version.

What can I do now?
------------------

Now you're ready to go and build something nice with help of our module extension. Perhaps you could check
[how to use login and sign up forms]() gracefully built for your needs, or you can check out how you could
[customize login form]() look'n'feel.
