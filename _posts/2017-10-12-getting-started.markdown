---
layout: post
title:  "Getting started"
date:   2017-10-12 12:00:00 +0200
categories: documentation
comments: false
author: "Dion Snoeijen"
---

{::options parse_block_html="true" /}
This is a simple step by step guide to get started with SexyField. We will use it with Symfony. For this guide we will build a simple blog.

# Step 1: Create a <a href="http://symfony.com/download" target="_blank">symfony</a> application.

Follow the steps described on the symfony website here: <a href="http://symfony.com/download" target="_blank">http://symfony.com/download</a>.

After running: `symfony new sexyblog` at your local dev environment we are ready to include SexyField.

Configure the database, and start the built in server with: `bin/console server:start`

<div class="info">
For a local database, you might just want to use docker.
Hereby a simple docker-compose config that will be sufficient to get started.

``` yaml
# docker/docker-compose.yml
version: "2"

services:
  mysql:
    image: mariadb
    ports:
      - 3306:3306
    environment:
      MYSQL_DATABASE: sexyblog
      MYSQL_ROOT_PASSWORD: S0m3FAk3PassW0rD
    volumes:
      - ./data/mysql:/var/lib/mysql
```
</div>

# Step 2: Add the sexy-field-bundle package to composer.

<div class="info">
At this moment, SexyField is still in development. Therefore we have to add two configurations to composer.json
`"minimum-stability": "dev",` and `"prefer-stable": true,`

``` json
{
    "name": "vendor/sexyblog",
    "license": "proprietary",
    "type": "project",
    "minimum-stability": "dev",
    "prefer-stable": true,
```
</div>

Add the sexy-field-bundle dependency to "require".

``` json
"tardigrades/sexy-field-bundle": "dev-master",
```

Let's use migrations to keep track of changes in the database. Also add:

``` json
"doctrine/doctrine-migrations-bundle": "^1.2.1",
```
And run composer update.

# Step 3: Run migrations

<div class="info">
SexyField configuration is stored in the database, a database scheme is present. For a SexyField doctrine setup the <a href="https://symfony.com/doc/master/bundles/DoctrineMigrationsBundle/index.html" target="_blank">doctrine migrations bundle</a> is recommended.
</div>

Run: `bin/console doctrine:migrations:diff` and `bin/console doctrine:migrations:migrate`

# Step 4: Install the field types

Execute the following command (just copy / paste):

``` bash
bin/console sf:install-field-type Tardigrades\\FieldType\\Choice\\Choice \
bin/console sf:install-field-type Tardigrades\\FieldType\\DateTime\\DateTimeField \
bin/console sf:install-field-type Tardigrades\\FieldType\\Email\\Email \
bin/console sf:install-field-type Tardigrades\\FieldType\\Integer\\Integer \
bin/console sf:install-field-type Tardigrades\\FieldType\\Number\\Number \
bin/console sf:install-field-type Tardigrades\\FieldType\\Relationship\\Relationship \
bin/console sf:install-field-type Tardigrates\\FieldType\\RichTextArea\\RichTextArea \
bin/console sf:install-field-type Tardigrates\\FieldType\\Slug\\Slug \
bin/console sf:install-field-type Tardigrades\\FieldType\\TextArea\\TextArea \
bin/console sf:install-field-type Tardigrades\\FieldType\\TextInput\\TextInput
```

# Step 5: Make configurations.

<div class="info">
In essence, SexyField is data storage agnostic. So configuration is based on how you want your data to be structured in general. You can look at it as in sections that contain fields. Just like a database that has columns, or a form has input fields. Based on this configuration, SexyField can automate several tasks. This particular bundle includes the following to support doctrine:
<br /><br />

`"tardigrades/sexy-field"` This is the base package.

`"tardigrades/sexy-field-api"` Provides endpoints based on sections.

`"tardigrades/sexy-field-entity"` Contains an entity generator and FieldType entity support.

`"tardigrades/sexy-field-doctrine"` Contains a doctrine config generator, a doctrine reader and a writer and FieldType doctrine config support.

`"tardigrades/sexy-field-form"` Integrates symfony forms.

`"tardigrades/sexy-field-field-types-base": "dev-master"` These are the provided field types.
</div>

In /app/config/sexy-field there is a folder that should
