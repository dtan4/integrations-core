[![Build Status][1]][2]
[![Build status][3]][4]

# Datadog Agent Integrations - Core

This repository contains the Agent Integrations (also known as checks) that Datadog
officially develops and supports. To add a new integration, please see the [Integrations Extras][5]
repository and the [accompanying documentation][6].

The [Datadog Agent][7] packages are equipped
with all the checks from this repository, so to get started using them, you can
simply [install the Agent][8] for your operating
system.

General documentation about the project can be [found here](docs/index.md)

## Integrations as Python wheels

When working with an integration, you will now be dealing with a more structured
python project. The new structure should help keep a more sane and modular codebase.
To help with the transition, please take a look at the following map to understand
where everything falls into place in the new approach.

| FORMER LOCATION | NEW LOCATION |
| --------------- | ------------ |
| {integration}/check.py | {integration}/datadog_checks/{integration}/{integration}.py |
| {integration}/conf.yaml.example | {integration}/datadog_checks/{integration}/conf.yaml.example |
| n/a | {integration}/datadog_checks/{integration}/\_\_init\_\_.py |
| {integration}/test_check.py | {integration}/test/test_{integration}.py |
| n/a | {integration}/test/\_\_init\_\_.py |
| n/a | {integration}/setup.py |

## A note about installing

Now that integrations are cleanly defined as python packages, we will soon be able
to ship them as Python wheels that will be pip-installable in the Python environment
embedded into the Datadog Agent. This presents a paradigm change in the way we will
be delivering standalone integration upgrades, moving away from OS-specific packages
to idiomatic Python package delivery.

Agent releases starting from version 5.21 bundle the latest wheels for any
integration, but at the moment you can't upgrade or downgrade between releases.

## Development

You can follow these instructions to get a working copy of any check on your
local Python environment; this is mostly useful to run tests or for tinkering in
general.

### Prerequisites

* Python 2.7, see [this page](docs/dev/python.md) for more details.

### Quickstart

The project comes with a requirements file you can pass to `pip` to install all
the dependencies needed to work with any check. From the root of the repo, run:

```shell
pip install -r requirements-dev.txt
```

To work with a specific check you need to install its own dependencies. The easiest
way to iterate on a check development is installing the wheel itself in editable mode.
For example, if you want to do this for the `disk` check run the following:

```shell
cd disk && pip install -e .
```

To double check everything is working as expected you can run:

```shell
python -c"from datadog_checks.disk import Disk"
```

if the commands ends without errors, you're good to go!

### Testing

To run the testsuite for a given check you can either use `tox`, like:

```shell
cd {integration} && tox
```

or invoke [Pytest][9] directly:

```shell
cd {integration} && py.test
```

**Note:** only a subset of the checks can be tested like this. Porting all the
checks to Pytest is a work in progress, this is the list of the checks supporting
the new testing approach:

* [apache](apache)
* [btrfs](btrfs)
* [directory](directory)
* [disk](disk)
* [elastic](elastic)
* [envoy](envoy)
* [istio](istio)
* [kube_proxy](kube_proxy)
* [kubelet](kubelet)
* [lighttpd](lighttpd)
* [linkerd](linkerd)
* [mcache](mcache)
* [network](network)
* [nfsstat](nfsstat)
* [postgres](postgres)
* [powerdns_recursor](powerdns_recursor)
* [prometheus](prometheus)
* [redisdb](redisdb)
* [spark](spark)
* [ssh_check](ssh_check)
* [system_core](system_core)
* [teamcity](teamcity)
* [vsphere](vsphere)

For checks that are not listed here, please refer to [Legacy development Setup](docs/dev/legacy.md).

If you updated the test requirements for a check, run `tox --recreate` for changes to be effective.

### Building

`setup.py` provides the setuptools setup script that will help us package and
build the wheel. If you wish to learn more about python packaging please take a
look at the official python documentation [here][10]

Once your setup.py is ready, creating a wheel is a easy as:

```shell
cd {integration}
python setup.py bdist_wheel
```

# Reporting Issues

For more information on integrations, please reference our [documentation][11]
and [knowledge base][12]. You can also visit our
[help page][13] to connect with us.


[1]: https://travis-ci.org/DataDog/integrations-core.svg?branch=master
[2]: https://travis-ci.org/DataDog/integrations-core
[3]: https://ci.appveyor.com/api/projects/status/8w4s2bilp48n43gw?svg=true
[4]: https://ci.appveyor.com/project/Datadog/integrations-core
[5]: https://github.com/DataDog/integrations-extras
[6]: https://docs.datadoghq.com/developers/integrations/integration_sdk/
[7]: https://github.com/DataDog/datadog-agent
[8]: https://docs.datadoghq.com/agent/
[9]: https://docs.pytest.org/en/latest/
[10]: https://packaging.python.org/tutorials/distributing-packages/
[11]: http://docs.datadoghq.com
[12]: https://help.datadoghq.com/hc/en-us
[13]: http://docs.datadoghq.com/help/
