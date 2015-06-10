## Notice

**Please use emqttd/plugins/emqttd_auth_mysql if your only need authentication with MySQL database.** 

##  Overview

Authentication, ACL with MySQL Database

## Usage

This project is a plugin of emqttd

In emqttd project:

```
git submodule add https://github.com/emqtt/emqttd_plugin_mysql.git plugins/emqttd_plugin_mysql
```

## etc/plugin.config

```erlang
[
{emysql, [
    {pool, 4},
        {host, "localhost"},
        {port, 3306},
        {username, "root"},
        {password, "root"},
        {database, "emqtt"},
        {encoding, utf8}
]},
{emqttd_plugin_mysql, [
    {users_table, auth_user},
    {acls_table, auth_acl},
    {field_mapper, [
        {username, username},
        {password, password, pbkdf2},
        {user_super, is_super_user},
        {acl_username, username},
        {acl_rw, rw},
        {acl_topic, topic}
    ]}
]}
].
```

## Users Table(Demo)

Notice: This is a demo table. You could authenticate with any user tables.

    ```
    CREATE TABLE auth_user (
            id int(11) NOT NULL AUTO_INCREMENT,
            password varchar(128) NOT NULL,
            is_superuser tinyint(1) NOT NULL,
            username varchar(30) NOT NULL,
            PRIMARY KEY (id),
            UNIQUE KEY username (username)
            ) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=utf8
    CREATE TABLE auth_acl (
            id int(11) NOT NULL AUTO_INCREMENT,
            topic varchar(100) NOT NULL,
            username varchar(30) NOT NULL,
            rw tinyint(1) NOT NULL,
            PRIMARY KEY (id)
            ) ENGINE=InnoDB AUTO_INCREMENT=66 DEFAULT CHARSET=utf8

    ```

## Load Plugin

    Merge the'etc/plugin.config' to emqttd/etc/plugins.config, and the plugin will be loaded by the  broker.

