# ATG *server workflow* demo

This repository collects together all of the necessary files to demonstrate how to deploy/use VectorCAST/ATG in a _server workflow_ (e.g., as part of a continuous integration system such as Jenkins).

**Importantly** if you are an 'end-user' of VectorCAST/ATG these examples are likely not to be relevant to you. VectorCAST's ATG technology can be used directly from either the VectorCAST/GUI or via `clicast`:

```bash
$VECTORCAST_DIR/clicast -lc -e <env> [-u <unit> [-s <sub>]] TOols AUTO_Atg_test_generation <outputfile>
```

If you require help or assistance with using VectorCAST or VectorCAST/ATG, we recommend you [contact Vector's support](mailto:support@vector.com).

## Initialising this repository

This repository uses `git` submodules to bring the complete demo together, to initialise this demo, please run the following:

```bash
# clone this repository
git clone https://github.com/VectorSoftwareRI/atg_demo.git

# enter the clone
cd atg_demo

# obtain all of the submodules
git submodule update --init --recursive
```

## Prerequisites

These demos have been prepared using [CentOS 8](https://www.centos.org/download/), and using [`gcc 8.3.1`](https://gcc.gnu.org/gcc-8/) and [Python 3.6.8](https://www.python.org/downloads/release/python-368/).

Prior to starting, we recommend you execute the dependency checker to ensure that your prerequisites match:

```bash
# Run the dependency checker (without checking a Manage project)
./atg_scripts/check_deps.sh -b
```

## Server scripts

The logic for executing VectorCAST/ATG in a server workflow is found in the `git` submodule [`atg_scripts`](atg_scripts). To allow a user to utilise these scripts without requiring direct installation, it is recommended to take advantage of Python's [virtual environments](https://docs.python.org/3/tutorial/venv.html) (`venv`). The dependencies for this project are listed in [`requirements.txt`](https://github.com/VectorSoftwareRI/atg_support_scripts/blob/master/requirements.txt).

**Prior to starting, you must ensure you have all of the dependencies for these scripts.** We refer you to the [support scripts README.md](https://github.com/VectorSoftwareRI/atg_support_scripts/blob/master/README.md).

Before continuing, please make sure that you have run:

```bash
# Setup the venv
./atg_scripts/setup_venv.sh
```

## Demos

We have included two "pre-configured" demos, with both source tress and VectorCAST/Manage projects:

* "small" -- this is a simple, hand-written demo that should be quick to execute and easy to understand
* "s2n" -- this is based off of [Amazon's s2n library](https://github.com/awslabs/s2n)

To run anything in `atg_scripts`, you must ensure you have the `venv` activated:

```bash
source atg_scripts/venv/bin/activate
```

Once you are finished, you can "leave" the `venv` by running:

```bash
deactivate
```

### Running the small demo

This example is a small, self-contained example, that is designed to quickly demonstrate VectorCAST/ATG's server workflow. The source code for this example can be found here:

* [`src`](https://github.com/VectorSoftwareRI/atg_small_demo_src/tree/master/src)

and the VectorCAST/ATG configuration files can be found here:

* [`small_demo/atg_configuration`](https://github.com/VectorSoftwareRI/atg_small_demo_vcast/tree/master/atg_configuration)

Prior to starting this demo, we recommend that you start VectorCAST/Manage and familiarise yourself with the project:

```bash
# go to the Manage project
cd small_demo/vcast

# set the required environment variables
source set_vcast_env.sh

# start VectorCAST/Manage
$VECTORCAST_DIR/vcastqt -e small.vcm
```

Including building some environments.

#### Generate all tests

To process *all* of the files inside of the small example, you should use [`small_demo/atg_configuration/small.py`](https://github.com/VectorSoftwareRI/atg_small_demo_vcast/blob/master/atg_configuration/small.py). This small configuration is not configured to run "incrementally", so all environments will be processed.

The demo can be run as follows:

```bash
./atg_scripts/atg_main.py -p ./small_demo/atg_configuration/small.py --verbose=True
```

After running this, we recommend that you manually re-open VectorCAST/Manage, execute the test-cases and look at the results.

#### Generate tests incrementally

This small example is also configured to be able to run a subset of files based on extracting change information from `git`. The configuration object ([`small_demo/atg_configuration/small.py`](https://github.com/VectorSoftwareRI/atg_small_demo_vcast/blob/master/atg_configuration/small.py)) interacts with `git` to find the files that have been changed between the two most recent comments.

To see the list of file changes (outside of running VectorCAST/ATG):

```bash
# enter the source directory for the small example
cd small_demo/src

# run git diff
git diff --name-only HEAD~1

# return to the root
cd ../..
```

You can now run this demo, including incremental processing:

```bash
./atg_scripts/atg_main.py -p ./small_demo/atg_configuration/small_incremental.py --verbose=True
```

After running this, we recommend that you manually re-open VectorCAST/Manage, execute the test-cases and look at the results.

### Running the s2n demo

This is a much larger example, that has been configured to demonstrate running VectorCAST/ATG on "real world" code. It is configured to automatically test Amazon's open-source cryptography library, [s2n](https://en.wikipedia.org/wiki/S2n); the name "s2n" is for "signal to noise".

The (official) source tree can be found here:

* [`s2n`](https://github.com/awslabs/s2n)

and the VectorCAST/ATG configuration files can be found here:

* [`s2n_demo/atg_configuration`](https://github.com/VectorSoftwareRI/atg_s2n_demo_vcast/tree/master/atg_configuration).

Prior to starting this demo, we recommend that you start VectorCAST/Manage and familiarise yourself with the project:

```bash
# go to the Manage project
cd s2n_demo/vcast

# set the required environment variables
source set_vcast_env.sh

# start VectorCAST/Manage
$VECTORCAST_DIR/vcastqt -e s2n.vcm
```

Including building some environments.

**Importantly** this demo is configured to use [`-m32`](https://gcc.gnu.org/onlinedocs/gcc/x86-Options.html) when compiling, so it requires the 32-bit development libraries for [OpenSSL](https://www.openssl.org).

#### Generate all tests

In a similar way to the small example, our first configuration file [`s2n.py`](https://github.com/VectorSoftwareRI/atg_s2n_demo_vcast/blob/master/atg_configuration/s2n.py) is setup to execute all environments inside of [`s2n.vcm`](https://github.com/VectorSoftwareRI/atg_s2n_demo_vcast/blob/master/vcast/s2n.vcm).

These tests can be generated as follows:

```bash
./atg_scripts/atg_main.py -p ./s2n_demo/atg_configuration/s2n.py --verbose=True
```

After running this, we recommend that you manually re-open VectorCAST/Manage, execute the test-cases and look at the results.

#### Generate tests incrementally

We have also provided an "incremental" configuration for s2n: [`s2n_incremental.py`](https://github.com/VectorSoftwareRI/atg_s2n_demo_vcast/blob/master/atg_configuration/s2n_incremental.py).

This incremental configuration runs VectorCAST on the changes from the s2n pull request [`#1780` -- "Fix the off-by-one on TLS1.3 record sizing"](https://github.com/awslabs/s2n/pull/1780).

To see the list of file changes (outside of running VectorCAST/ATG):

```bash
# enter the source directory for the small example
cd s2n_demo/src

# run git diff
git diff --name-only 4caa406~1 4caa406

# return to the root
cd ../..
```

You can now generate tests incrementally as follows:

```bash
./atg_scripts/atg_main.py -p ./s2n_demo/atg_configuration/s2n_demo.py --verbose=True
```

After running this, we recommend that you manually re-open VectorCAST/Manage, execute the test-cases and look at the results.

