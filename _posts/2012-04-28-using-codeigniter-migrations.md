---
layout: post
title: "Using CodeIgniter Migrations"
description: ""
category:
tags: []
---


## About

Migrations give you the ability keep your database across all environments the same without comprimising the data
inside the database. Think it as a version control for your database schema. You should be doing this at all times
especially when there are multiple developers so all developers don't need to worry about having the latest database
table.

## Step by step

- You first need to create your migrations folder, `application/migrations/`.
- Let us start our first migration file.

`application/migrations/001_create.php`
{% highlight php %}
<?php
class Migration_Create extends CI_Migration {

	public function up()
   {
		$fields = array(
			'session_id' => array(
				'type' => 'VARCHAR',
				'constraint' => '32',
			),
			'user_agent' => array(
				'type' => 'VARCHAR',
				'constraint' => '255',
				'null' => true,
			),
			'ip_address' => array(
				'type' => 'VARCHAR',
				'constraint' => '20',
				'null' => true,
			),
			'last_activity' => array(
				'type' => 'INT',
				'constraint' => '12',
				'null' => true,
			),
			'user_data' => array(
				'type' => 'TEXT',
			),
		);

		$this->dbforge->add_field($fields);
		$this->dbforge->add_key('session_id', TRUE);
		$this->dbforge->create_table('ci_sessions');
   }


   public function down()
   {
      $this->dbforge->drop_table('ci_sessions');
   }

}
{% endhighlight %}

You will see that you are extending a migration class that is creating the sessions table. You can see that for each Array() in fields array it imitates a column. Everytime a migration is being upstreamed, CodeIgniter will call upon `up()` and on downstream it will call `down()`.

- Once your first migration is complete, you need to edit `application/config/migration.php` with the proper configuration variables set.

`$config['migration_enabled'] = TRUE;`

`$config['migration_version'] = 1;`

As you see here, you need to set `migration_version` everytime you create a new migration file.

- Last step! Almost there! Now we actually have to call the actual migration. In your model or controller, you need to add the following code.

{% highlight php %}
$this->load->library('migration');
$this->migration->current();
{% endhighlight %}

You should note that you should only call this code block that requires the specific schema of the database.
