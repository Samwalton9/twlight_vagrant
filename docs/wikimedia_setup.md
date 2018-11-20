# Setting up TWLight in the Wikimedia Cloud VPS environment

This guide is an overview of how to setup the Library Card platform for use in the Wikimedia Cloud VPS environment, where the production site is currently hosted.

# Access

To follow the rest of this guide you must first be a project administrator for the [twl project](https://tools.wmflabs.org/openstack-browser/project/twl). Contact an existing administrator if you require access.

# Process

## Horizon instance

For general information on using Horizon see the [Horizon FAQ](https://wikitech.wikimedia.org/wiki/Help:Horizon_FAQ).

1. If you are recreating an instance, first delete the old one in the Compute > Instances tab, taking note of the previous name.
2. Launch a new instance, entering the desired hostname (e.g. `twlight-staging`), and creating an `m1.medium` instance on `debian-8.11-jessie`.
3. Click on the new instance, and scroll to the bottom of the Puppet Configuration tab. Edit the Hiera Config to contain only `mount_nfs: true` to give this instance access to the project NFS share.
4. Create a web proxy by navigating to the DNS > Web Proxies tab and creating a new proxy to the newly created backend instance. The backend port should be set to 443.

## Connect

If this is your first time connecting to a Wikimedia Cloud Services environment, see [Help:Access](https://wikitech.wikimedia.org/wiki/Help:Access) for guidance on setting up SSH keys and config.

You should now be able to SSH to the instance with `ssh hostname.twl.eqiad.wmflabs`, replacing `hostname` as appropriate. If you are recreating an instance you may need to drop your old keys.

The instance should have access to `/data/project` (the NFS share), in which you can find the setup script `init.sh`. If this is missing for any reason, it can be restored from [this gist](https://gist.github.com/jsnshrmn/02493eb679427932174eff14faa66b67). The script makes local data directories, links to the NFS share, installs relevant modules and applies them.

This script automatically loads the appropriate .yaml configuration file from `/data/project/puppet/data/nodes/`. If you're using a hostname which isn't listed there you will need to create a new one. If recreating an instance, such as twlight-prod or twlight-staging, a config should already be present and the script will automatically detect it.

Once the script has finished running you should now be able to access the tool at the hostname you set up for the web proxy!
