---
layout: post
title:  "Getting started with SexyField"
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
<h1 id="step-5"><a href="#step-5">Step 5: Make configurations</a></h1>

* <a href="#language">Language</a>
* <a href="#application">Application</a>
* <a href="#field">Field</a>
* <a href="#section">Section</a>

Configurations are done with yml files. At this point we have a couple of different configurations.

<div class="info">
The resulting folder structure will look like this.

In app/config:
* sexy-field
  * application
    * application.yml
    * language.yml
  * author
    * field
      * firstName.yml
      * infix.yml
      * lastName.yml
    * author.yml
  * blog
    * field
      * title.yml
      * summary.yml
      * author.yml
    * blog.yml
  * comment
    * field
      * name.yml
      * email.yml
      * comment.yml
    * comment.yml
  * generic
    * field
      * created.yml
      * updated.yml
</div>

There are commands available to apply configurations.

<div class="info">
Application commands<br />

`bin/console sf:create-application <path to config yml>`

`bin/console sf:update-application <path to config yml>`

`bin/console sf:delete-application (follow dialog)`

`bin/console sf:list-application`


<br />Language commands<br />

`bin/console sf:create-language <path to config yml>`

`bin/console sf:update-language <path to config yml>`

`bin/console sf:delete-language (follow dialog)`

`bin/conole sf:list-language`


<br />Field type commands<br />

`bin/console sf:instal-field-type <namespace> (Escape \ in namespace)`

`bin/console sf:update-field-type (follow dialog)`

`bin/console sf:delete-field-type (follow dialog)`

`bin/console sf:list-field-type`


<br />Field commands<br />

`bin/console sf:create-field <path to config yml>`

`bin/console sf:update-field <path to config yml> (follow dialog)`

`bin/console sf:delete-field (follow dialog)`

`bin/console sf:list-field`


<br />Section commands<br />

`bin/console sf:create-section <path to config yml>`

`bin/console sf:update-section <path to config yml> (follow dialog)`

`bin/console sf:delete-section (follow dialog)`

`bin/console sf:list-section`

`bin/console sf:generate-section (follow dialog)`

`bin/console sf:restore-section (follow dialog)`

</div>

<h1 id="language"><a href="#language">Language</a></h1>

For section entries, you can have multiple languages.

<div class="info">
At this point, language functionality is incomplete, don't bother too much, just create the languages.
</div>

``` yml
# app/config/sexy-field/application/language.yml
language:
  - nl_NL
  - en_EN
```

From the root of your project, run:<br />
`bin/console sf:create-language app/config/sexy-field/application/language.yml`

<h1 id="application"><a href="#application">Application</a></h1>

You can have multiple applications that can relate to sections, you can look upon this as a group of sections.

<div class="info">
At this point, application functionality is incomplete, don't bother too much, just create the application.
</div>

``` yml
# app/config/sexy-field/application/application.yml
application:
  name: Blog
  handle: blog
  languages:
    - nl_NL
    - en_EN
```

From the root of your project, run:<br />
`bin/console sf:create-application app/config/sexy-field/application/application.yml`

<h1 id="field"><a href="#field">Field</a></h1>

> Sections are created from fields that are based on field types.

A field can be assigned to multiple sections. For some fields this is relevant. Like the updated and created field. It's recommended to have all sections contain those two fields. Let's start by creating both.

``` yml
# app/config/sexy-field/generic/field/created.yml
field:
  name: Created
  handle: created
  type: DateTimeField
  entityEvents:
    - prePersist
  form:
    all:
      label: Creation date
```
``` yml
# app/config/sexy-field/generic/field/updated.yml
field:
  name: Updated
  handle: updated
  type: DateTimeField
  entityEvents:
    - prePersist
    - preUpdate
  form:
    all:
      label: Update date
```

name: This is the field name that can be displayed for interal, admin users.
handle: The handle is usually based on the name. It is used to access the field data when in a section.
type: Refers to the field type that is being configured.

<div class="info">
entityEvents will be deprecated soon and change: [https://github.com/dionsnoeijen/sexy-field/issues/17](https://github.com/dionsnoeijen/sexy-field/issues/17)
</div>

The entity events are telling the entity generator what event generators it should trigger. At this point, this particular field type (DateTimeField) supports two event generators through the `sexy-field-entity` package: EntityPrePersistGenerator and EntityPreUpdateGenerator.

If you add them to the array, like in the example. The entity generator will generate the accompanying event handling methods like this:

``` ruby
public function onPrePersist(): void
{
    $this->created = new \DateTime('now');
    $this->updated = new \DateTime('now');
}

public function onPreUpdate(): void
{
    $this->updated = new \DateTime('now');
}
```

The `form` config, points to how it should render the form.

It has three groups, in this case only one is used.

`all` For both create and update forms.<br />
`create` Config that only works for the create form.<br />
`update` Config that only works for the update form.<br />

<h1 id="section"><a href="#section">Section</a></h1>
