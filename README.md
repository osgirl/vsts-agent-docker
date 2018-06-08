# Visual Studio Team Services agent

This repository contains [docker images](https://hub.docker.com/r/emisgroup/vsts-agent-docker/) for the Visual Studio Team Services (VSTS) agent that runs tasks as part of a build or release.

## Supported tags and Dockerfile links

* ubuntu-16.04-php-node
	* PHP 7.2 with composer
	* Node.js 10.0 with npm 6.0
	* Grunt-Cli
    * xsltproc so you can convert your PHPUnit output to [something VSTS will support](https://github.com/sebastianbergmann/phpunit/issues/2964)

* windowsservercore-1709
	* .NET Framework 4.7.1
	* VS 2017 Build Tools 15.6.7


## How to use these images

```
docker run \
    -e VSTS_ACCOUNT=<team name> \
    -e VSTS_POOL="<pool>" \
    -e VSTS_TOKEN="<pat>" \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -it \
    emisgroup/vsts-agent-docker:<tag>
```

You can use the additional parameter ``` -e SERVER_LANGUAGE=en-GB ``` on the windows image to change the language and culture of the container
