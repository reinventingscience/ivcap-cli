# Development has moved to [github.com/ivcap-works/ivcap-cli](https://github.com/ivcap-works/ivcap-cli)

# IVCAP - A Command-line Tool to interact with an IVCAP deployment

__IVCAP__ is helping researchers better investigate their domains and derive new insights  by collecting, processing and analysing multi-modal and multi-scale data and better facilitating data provenance and thereby fostering interdisciplinary collaboration.

__IVCAP__ has an extensive REST API which is usually called directly from applications or scientific notebooks. However, to support simple data operation from the command line, we developed this simple command-line tool. It only covers the subset of the IVCAP API, but we would be very excited to receive pull requests to extend it's functionality or fix bugs.

## Install

There are [ready to use binaries](https://github.com/ivcap-works/ivcap-cli/releases/latest) for some architectures available at the repo's [release](https://github.com/ivcap-works/ivcap-cli/releases) tab.

If you use [homebrew](https://brew.sh/), you can install it by:

```
brew tap brew tap ivcap-works/ivcap
brew install ivcap
```

## Usage

```

% ivcap
A command line tool to to more conveniently interact with the
API exposed by a specific IVCAP deployment.

Usage:
  ivcap [command]

Available Commands:
  artifact    Create and manage artifacts 
  completion  Generate the autocompletion script for the specified shell
  context     Manage and set access to various IVCAP deployments
  help        Help about any command
  login       Authenticate with a specific deployment/context
  logout      Remove authentication tokens from specific deployment/context
  metadata    Add/get/revoke metadata
  order       Create and manage orders 
  service     Create and manage services 

Flags:
      --access-token string   Access token to use for authentication with API server [IVCAP_ACCESS_TOKEN]
      --context string        Context (deployment) to use
      --debug                 Set logging level to DEBUG
  -h, --help                  help for ivcap
  -o, --output string         Set format for displaying output [json, yaml]
      --silent                Do not show any progress information
      --timeout int           Max. number of seconds to wait for completion (default 10)
  -v, --version               version for ivcap

Use "ivcap [command] --help" for more information about a command.
```

### Configure context for a specific deployment

With the following command we are creating a context named `cip-2` for the IVCAP deployment at `https://api2.green-cirrus.com`

```
% ivcap context create cip-2 https://api2.green-cirrus.com
Context 'cip-2' created.
```

If we have multiple contexts, we can easily switch with `context set`

```

% ivcap context set cip-2
Switched to context 'cip-2'.
```

The following command lists all the configured contexts:

```
% ivcap context list
+---------+-----------+-----------------------------+--------------------------------+
| CURRENT | NAME      | ACCOUNTID                   | URL                            |
+---------+-----------+-----------------------------+--------------------------------+
| *       | cip-2     | urn:ivcap:account:4c65b865  | <http://api2.green-cirrus.com> |
+---------+-----------+-----------------------------+--------------------------------+
```

To obtain an authorisation token, some deployments provide a username/password based identity provider.

```
% ivcap login
                                         
                                         
    █▀▀▀▀▀█    ▀█  ▄▀▄▀▀ ▄▄▀▄ █▀▀▀▀▀█    
    █ ███ █ █  █▀ ▀█▀ █  ▀▀█  █ ███ █    
    █ ▀▀▀ █ █ ▀▀▄▀▀▀▀█▀ ▀█ ▀▀ █ ▀▀▀ █    
    ▀▀▀▀▀▀▀ █▄█ █ █ █ █ █▄█ ▀ ▀▀▀▀▀▀▀    
    █ ██▀▀▀▄▄▄ ▄ ██ ██▄█▀▄█▄█ ██▀██ ▄    
    █▀▄▄ ▀▀  █ █▀█▀▀▀█▄  █  █ ▄ █▄█▀     
...
To login to the IVCAP Service, please go to:  https://id-provider.com/activate?user_code=....
or scan the QR Code to be taken to the login page
Waiting for authorisation...
```

### Services

To list all available services:

```

% ivcap services list --limit 2
+--------------------------------------------------------+---------------------+------------------------------+
| ID                                                     | NAME                | PROVIDER                     |
+--------------------------------------------------------+---------------------+------------------------------+
| urn:ivcap:service:d939b74d-0070-59a4-a832-36c5c07e657d | Gradient Text Image | urn:ivcap:provider:1a18fe... |
| urn:ivcap:service:74856672-26f1-5df8-9de0-787f6a1fed25 | Windy days.         | urn:ivcap:provider:48609f... |
+--------------------------------------------------------+---------------------+------------------------------+

```

To get more details about a specific service

```

% ivcap service get urn:ivcap:74856672-26f1-5df8-9de0-787f6a1fed25

          ID  urn:ivcap:service:74856672-26f1-5df8-9de0-787f6a1fed25                        
        Name  Windy days.                                                              
 Description  The number of days with average near-surface wind speed above threshold.

              Let $WS_{ij}$ be the windspeed at day $i$ of period $j$. Then            
              counted is the number of days where:                                     
                                                                                       
              $$                                                                       
                  WS_{ij} >= Threshold [m s-1]                                         
              $$                                                                       

 Provider ID  urn:ivcap:provider:48609f7d-5a64-5bf6-9c47-1a3ad40bf28a:cre.csiro.au
  Account ID  urn:ivcap:account:6b1d01e0-c2c9-5448-b8a2-9fc14ac18b55:cre.csiro.au
  Parameters  ┌───────────────┬────────────────────────────────┬────────┬────────────┐
              │ NAME          │ DESCRIPTION                    │ TYPE   │ DEFAULT    │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │        thresh │ Threshold average near-surface │ string │ 10.8       │
              │               │  wind speed on which to base e │        │            │
              │               │ valuation.                     │        │            │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │          freq │ Resampling frequency.          │ option │ MS         │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │ degrees_north │ Latitude: Degrees North        │ number │ -10.3      │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │ degrees_south │ Latitude: Degrees South        │ number │ -45        │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │  degrees_east │ Longitude: Degrees East        │ number │ 115        │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │  degrees_west │ Longitude: Degrees West        │ number │ 110        │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │         model │ Climate Model                  │ option │ ACCESS1.3  │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │    experiment │ Emissions Pathway              │ option │ historical │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │    start_date │ Start date (YYYY-MM-DD)        │ string │ 1975-01-01 │
              ├───────────────┼────────────────────────────────┼────────┼────────────┤
              │      end_date │ Start date (YYYY-MM-DD)        │ string │ 2005-12-31 │
              └───────────────┴────────────────────────────────┴────────┴────────────┘

```

### Orders

To place an order:

```

% ivcap orders create \
     urn:ivcap:service:d939b74d-0070-59a4-a832-36c5c07e657d \
     --name "Order for max" \
     msg="Hi, how are you"
Order 'urn:ivcap:order:81b204e8-c404-499e-bc19-d78518a5a3dc' with status 'Pending' submitted.

```

To check on the status of an order:

```
% ivcap orders get urn:ivcap:order:81b204e8-c404-499e-bc19-d78518a5a3dc

         ID  urn:ivcap:order:81b204e8-c404-499e-bc19-d78518a5a3dc                        
       Name  Order for max                                                          
     Status  Succeeded                                                              
 Ordered at  02 Jun 22 16:10 AEST
 Service ID  urn:ivcap:service:d939b74d-0070-59a4-a832-36c5c07e657d
 Account ID  urn:ivcap:account:58d8e161-9a2b-513a-bd32-28d7e8af1658:testing.com
 Parameters  ┌────────────────────────┐
             │ msg =  Hi, how are you │
             └────────────────────────┘
   Products  ┌────────────────────────────────────────────────-----────┬─────────┬───────┐
             │ urn:ivcap:artifact:017ecae8-3d39-4297-a94f-00ddf9b26611 │ out.png │ 50855 │
             └───────────────────────────────────────────-----─────────┴─────────┴───────┘
```

### Artifacts

To check the details of the artifact created by the previously placed order:

```

% ivcap artifact get urn:ivcap:artifact:017ecae8-3d39-4297-a94f-00ddf9b26611

         ID  urn:ivcap:artifact:017ecae8-3d39-4297-a94f-00ddf9b26611            
       Name  out.png                                                       
     Status  available                                                     
       Size  50855                                                         
  Mime-type  image/png
 Account ID  urn:ivcap:account:58d8e161-9a2b-513a-bd32-28d7e8af1658:testing.com

```

To download the content associated with the artifact.

```

% ivcap artifact download urn:ivcap:artifact:017ecae8-3d39-4297-a94f-00ddf9b26611 -f /tmp/out.png
Successfully wrote 50855 bytes to /tmp/out.png
```
