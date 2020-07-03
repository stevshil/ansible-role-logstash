# Logstash Ansible Role

Ansible role to configure Logstash using templates and variables.  The aim of this project is to create your inputs, outputs and filters through creating Ansible variables.  It is intended to be highly configurable without having to write any of the files.

# Requirements

Logstash requires a version of Java to be installed.  You should use geerlingguy.java module for this.

You should also have a variable set called **java_packages** based on the name of the java package to install.

# Role variables

All the variables required can be found in the defaults/main.yml file.

The main variables for configuring logstash files are;
* logstash_inputs
  - This variable is an array that requires 2 parts in each array element;
    - **name** to define the logstash input plugin that will be used
    - **config** which will then allow you to configure the attributes for that plugin
      - Each line is a key: value pair, with the key being the logstash attribute to configure and the value being the setting you need
* logstash_outputs
  - This is also an array that requires 2 parts, just like the logstash_inputs;
    - **name** to define the logstash output plugin to use
    - **config** an optional element, but like logstash_inputs config requires you to supply the attributes to configure the plugin as key: value pairs
* logstash_filters
  - This is a more complex dictionary;
    - **use** must be set to **yes** or **no** (default) = no filter file
    - **config** as in the inputs and outputs is an array of filter plugins which requires
      - **name** of the logstash filter plugin to configure
      - **attrs** is a key: value pair list of attributes to configure for the plugin

Other variables to configure logstash to work on a systems;

- logstash_version: 7.8.0
  - The version of logstash to install
- logstash_user: root
  - Which user the logstash process should run as.  Sometimes you just need root for those logfiles
- logstash_group: root
  - Which group the logstash process will launch as
- journaldLimitBurst: 200000
  - To prevent journald from being overloaded and preventing logstash from reading log files
- elasticsearch_http_port: 9200
  - Which port elasticsearch is running on

Our example default/main.yml contains a coded method to locate the elasticsearch server in the default logstash_outputs variable.  It required a host group called **elk** to exist to pick up the server name.

## Dependencies

* geerlingguy.java
* Operating systems that support systemd

## License

MIT / BSD

## Author Information

This role was created in 2020 by [Steve Shilling](https://uk.linkedin.com/in/steve-shilling-01120b7), of [TPS Services Ltd](https://www.therapypages.com).
