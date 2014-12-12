nodeapp
==========

This role runs multiple instances of a node.js application managed by upstart.

Requirements
------------

Platform / ansible requirements are listed in the metadata file.

Role Variables
--------------

You must specify a `nodeapp_index` that points to the application script to run:

    nodeapp_index: /var/www/app.js

All other variables are optional, defaulting to:

    nodeapp_name: webapp
    nodeapp_node_version: 0.10.33
    nodeapp_num_workers: 3

These variables define an upstart service named "webapp" that, when started or
stopped:

    $ start webapp

...will launched three instances of the app. You may also optionally specify a
`nodeapp_env` variable whose contents will be set within each application
process's environment.

    nodeapp_env:
      - NODE_ENV: production
        PORT: "`printf '32%02i' $NODEAPP_INDEX`"

The variable `$NODEAPP_INDEX` is set inside each upstart instance with a unique
ID of the worker application.

Dependencies
------------

nodesource.node

Example Playbook
----------------

    - hosts: all
      roles:
        - role: rjz.nodeapp
          nodeapp_index: /var/www/app.js
          nodeapp_num_workers: 10

License
-------

MIT

