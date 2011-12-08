Capistrano's default recipe "deploy.rb":http://github.com/capistrano/capistrano/blob/master/lib/capistrano/recipes/deploy.rb expects a number of default roles to be defined so that it may operate correctly.

|^. <code>:web</code> | This is assumed to be your web servers (read: Apache / nginx / etc.) |
|^. <code>:app</code> | These are assumed to be your application servers, in a Ruby environment this means something like Mongrel, or Mongrel Cluster; it can also be your Passenger servers. |
|^. <code>:db</code> | This, naturally enough is your database server. You can use server/role modifiers below to control which is considered to be the primary one. |

h2. Populating Roles

<pre>
role :app, 'app1.example.com'
role :app, ['app2.example.com', 'app3.example.com']
role :web, 'web1.example.com'
</pre>

Notice that the role() method can take an array. You can read more in the API documentation "here":http://rdoc.info/rdoc/capistrano/capistrano/blob/515db3a0dec421d1c2200316ab58d7704176d577/Capistrano/Configuration/Roles.html#role-instance_method.

h2. Modifiers

The default recipe knows about two **modifiers**. That's to say extra configuration options that can be passed to role() to be used when specifying tasks.

|^. <code>:primary</code> | <code>true</code> or <code>false</code>. Used by the database related tasks to determine where to run migrations |
|^. <code>:no_release</code> | <code>true</code> or <code>false</code> Used by the code checkout tasks to determine where to install the code. Anything set <code>:no_release</code> will not checkout the code from the repository. |

The modifiers can be set in the following way:

<pre>
role :db, 'db1.example.com', :primary => true, :no_release => false
role :db, %w{db2.example.com db3.example.com}, :no_release => true
</pre>

In the above example <code>db1</code> will receive the code during the checkout due to <code>:no_release => false</code> (which is the default, or a no-op at least if it's not true). Due to the <code>:primary => true</code> modifier the database migrations will be run here.

On the other <code>db</code> servers, no migrations will take place, and no code will be released. This is perfect for isolating your database slaves away from any code responsibility, or having to have a Ruby stack installed.

h2. Defining your own roles.

You can select any arbitrary name which isn't already reserved for your role names. The default recipe will ignore them but when writing your own tasks you can define your own modifiers and role names

h2. Alternative Syntax

role() can also take a block, it is expected to return the options hash that would otherwise be expected.

In addition to role() there is also "server()":http://rdoc.info/rdoc/capistrano/capistrano/blob/515db3a0dec421d1c2200316ab58d7704176d577/Capistrano/Configuration/Roles.html#server-instance_method which is for defining an individual server, and specifying a role; there are circumstances when this is preferable.

h2. Servers playing multiple roles.

It is perfectly acceptable to have servers playing multiple roles:

<pre>
role :app, 'example.com'
role :web, 'example.com'
role :db, 'example.com', :primary => true
</pre>

However this is a nice use-case for the server() method, as you could simply write

<pre>
server "example.com", :web, :app
</pre> 