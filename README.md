# Merged Into MassBrowser
CacheBrowser has been merged into our new censorship circumvention tool called <b>MassBrowser</b>. Please visit the [MassBrowser project's website](https://massbrowser.cs.umass.edu) and its [git repository](https://github.com/SPIN-UMass/MassBrowser).

# CacheBrowser

The current CacheBrowser software is a research prototype implementation, and is not yet ready for end-users. If you are a researcher or developer check it out and give us feedback, otherwise please be patient while we work on an end-user version of the code. 
Thanks for your understanding and support! 

https://cachebrowser.net

- [Installing CacheBrowser](#installation)
- [Bootstrapping](#bootstrapping)
- [Commands](#cachebrowser-commands)

## Installation
```
pip install cachebrowser
```

## Running CacheBrowser
To run the CacheBrowser process simply enter the `cachebrowser` command
```
cachebrowser
```

You can then use CacheBrowser by using the CacheBrowser plugin for your browser or by setting the HTTP and HTTPS proxy on your browser to `localhost:8080`

## Bootstrapping
In order to be able to browse a page using CacheBrowser, the page must first be bootstrapped.
What this means is that CacheBrowser must first find out what CDN the page is hosted on and also obtain some edge server addresses for that CDN. 

CacheBrowser supports local and remote bootstrapping sources. Local bootstrapping sources are files in YAML format which provide bootstrapping information for hosts and CDNs. Remote bootstrapping sources are RESTful services which provide bootstrapping information through HTTP requests.


### Configuring Bootstrapping Sources
Any number of local or remote bootstrapping sources may be defined in the CacheBrowser configuration file. When bootstrapping a host or host, CacheBrowser will go through the provided sources in the specified order until the information is found.

Bootstrapping sources can be specified in the configuration file using the `bootstrapper_sources` key, for example:
```
boostrapper_sources:
	- type: local
	  path: local_bootstrapper.yaml
	- type: remote
	  url: https://bootstrap.cachebrowser.net/api
```

### Local Boostrapping Files
A local bootstrapping file is a YAML file containing a list of Host or CDN information.

For example:
```
- type: cdn
  id: akamai
  name: Akamai
  edge_servers:
    - 23.208.91.198
    - 23.218.210.7

- type: host
  name: www.nbc.com
  ssl: false
  cdn: akamai

- type: host
  name: www.bloomberg.com
  ssl: false
  cdn: akamai
```

For each **CDN** provided in the bootstrapping file the following information should be provided:

Key                        | Value 
---------------------------| ---
type                       | cdn
id                         | A unique ID to be used for the CDN
name                       | The CDN Name
edge_servers               | List of edge server addresses for the CDN

For each **Host** provided in the bootstrapping file the following information should be provided:

Key                        | Value 
---------------------------| ---
type                       | host
name                       | The hostname
cdn                        | The ID of the CDN which provides this host
ssl                        | Whether the pages supports HTTPS connections or not


## CacheBrowser Commands


You could also run a command with CacheBrowser:
```
cachebrowser <command>
```



These are the valid commands you could give:

Command                                                                     | Description 
--------------------------------------------------------------------------- | ---
bootstrap <host>                                                            | Bootstrap <host>
get `url` `[target]`                                                        | Retrieve `url` using CacheBrowser. If `target` is specified CacheBrowser uses the given target. If not, CacheBrowser uses the bootstrapped target if the host has been bootstrapped or makes a normal request to the host if not.
host list                                                                   | Lists the bootstrapped hosts
host add 								    | Add host info
cdn list                                                                    | Lists the bootstrapped CDNs 
cdn add 								    | Add host info


### Example
```
cachebrowser bootstrap www.nbc.com
cachebrowser get http://www.nbc.com

cachebrowser get https://www.istockphoto.com 69.31.76.91
```

