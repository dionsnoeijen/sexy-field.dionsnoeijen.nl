---
layout: post
title:  "Getting started"
date:   2017-10-12 12:00:00 +0200
categories: documentation
comments: true
author: "Dion Snoeijen"
---

This is a simple step by step guide to get started with SexyField. We will use it with Symfony. For this guide we will build a simple blog.

# Step 1: Create a <a href="http://symfony.com/download" target="_blank">symfony</a> application.

Follow the steps described on the symfony website here: <a href="http://symfony.com/download" target="_blank">http://symfony.com/download</a>.

After running: `symfony new sexyblog` at your local dev environment we are ready to include SexyField.

Configure the database, and start the built in server with: `bin/console server:start`

{::options parse_block_html="true" /}
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

{::options parse_block_html="true" /}
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

Let's use migrations for changes. Also add:

``` json
"doctrine/doctrine-migrations-bundle": "^1.2.1",
```

And run composer update.

# Step 3: Install the field types

{::options parse_block_html="true" /}
<div class="info">
This will change soon. Probably the fields will be configured automatically, without the need to install them.
</div>

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

