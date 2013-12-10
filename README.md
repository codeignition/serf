Serf Cookbook
=============

Installs and configures [Serf](http://www.serfdom.io/).

Supports
--------

 * Serf >= 0.2 (uses the config file option)
 * Linux OS'es

Usage
-----

Using the default attributes will setup a single Serf agent in its own cluster.

If you already have a Serf agent (or cluster) running specify the array of address(es) with the 
`node["serf"]["agent"]["start_join"]` attribute so the agent will join the cluster(s).

What does the installation look like
------------------------------------

By default the installation will look like,

    /opt/serf/* - All of serf's files (config, binaries, event handlers, logs...)
    /etc/serf/* - Link to all of serf's config files
    /var/log/serf - Link to all of serf's log files
    /etc/init.d/serf - An init.d script to start/stop the agent

Event Handlers
--------------

An [event handler](http://www.serfdom.io/docs/agent/event-handlers.html) is a script that is run when the Serf agent
recieves an event (member-join, member-leave, member-failed, or user).

You can configure an event handler via the attribute `node["serf"]["event_handlers"]`. The format for configuring an 
event handler throught the serf cookbook is,

    {
      "url" : "URL", # REQUIRED
      "event_type" : "EVENT_TYPE" #OPTIONAL
    }
    
The `event_type` value filters the event handler for certain events. Use [this doc](http://www.serfdom.io/docs/agent/event-handlers.html) 
to figure out the `event_type` you need.

It is also possible to add event handlers via the attribute `node["serf"]["agent"]["event_handlers"]`. The `node["serf"]["event_handlers"]`
helps with the deployment of the event handler file itself and will add the event handler to the `node["serf"]["agent"]["event_handlers"]`
attribute.

Attributes
----------

 * `node["serf"]["agent"][*]` : A hash of key/values that will be added to the agent's config file (default={}). Use [this doc](http://www.serfdom.io/docs/agent/options.html) to configure the agent.
 * `node["serf"]["event_handlers"]` : An array of hashes that represent [event handlers](http://www.serfdom.io/docs/agent/event-handlers.html). See 'Event Handlers' below for more details (default=[])
 * `node["serf"]["base_binary_url"]` : The base url used to download the binary zip (default="https://dl.bintray.com/mitchellh/serf/")
 * `node["serf"]["version"]` : The version of the Serf agent to install (default="0.2.1")
 * `node["serf"]["arch"]` : The architecture of the Serf agent to install (default=`kernel['machine'] =~ /x86_64/ ? "amd64" : "386"`)
 * `node["serf"]["binary_url"]` : The full binary url of the Serf agent (default=`File.join node["serf"]["base_binary_url"], "#{node["serf"]["version"]}_linux_#{node["serf"]["arch"]}.zip"`)
 * `node["serf"]["base_directory"]` : The base directory Serf should be installed into (default="/opt/serf")
 * `node["serf"]["log_directory"]` : The directory of the Serf agent logs (default="/var/log/serf")
 * `node["serf"]["confdirectory"]` : The directory of the Serf agent config (default="/etc/serf")
 