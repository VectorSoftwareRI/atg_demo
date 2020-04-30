# ATG *server workflow* demo

This repository collects together all of the necessary files to demonstrate how to deploy/use VectorCAST/ATG in a *server workflow*.

**Importantly** if you are an 'end-user' of VectorCAST/ATG these examples are likely not to be relevant to you. VectorCAST's ATG technology can be used directly from either the VectorCAST/GUI or via `clicast`:

```bash
$VECTORCAST_DIR/clicast -e <env> TOols AUTO_Atg_test_generation <outputfile>
```

If you require help or assistance with using VectorCAST or VectorCAST/ATG, we recommend you [contact Vector's support](mailto:support@vector.com).

## Initialising this repository

This repository uses `git` submodules to bring the complete demo together, to initialise this demo, please run the following:

```bash
https://github.com/VectorSoftwareRI/atg_demo.git
git submodule update --init --recursive
```

## Prerequisites

These demos have been prepared using [CentOS 8](https://www.centos.org/download/), and using [`gcc 8.3.1`](https://gcc.gnu.org/gcc-8/) and [Python 3.6.8](https://www.python.org/downloads/release/python-368/).

Prior to starting, we recommend you execute the dependency checker to ensure that your prerequisites match:

```bash
./atg_scripts/check_deps.sh
```

## Server scripts

The logic for executing VectorCAST/ATG in a server workflow is found in the `git` submodule [`atg_scripts`](atg_scripts). 

**Prior to starting, you must ensure you have all of the dependencies for these scripts.** We refer you to the [support scripts README.md](atg_scripts/README.md).

## Demos

We have included two "pre-configured" demos, with both source tress and VectorCAST/Manage projects:

* "small" -- this is a simple, hand-written demo that should be quick to execute and easy to understand
* "s2n" -- this is based off of [Amazon's s2n library](https://github.com/awslabs/s2n)

To run anything in `atg_scripts`, you must ensure you have the `venv` activated:

```bash
source atg_scripts/venv/bin/activate
```

### Running the small demo

#### Generate all tests

```
./atg_scripts/atg_main.py -p ./small_demo/atg_configuration/small.py --verbose=True
```

#### Generate tests incrementally

```
./atg_scripts/atg_main.py -p ./small_demo/atg_configuration/small_incremental.py --verbose=True
```

### Running the s2n demo

#### Generate all tests

```
./atg_scripts/atg_main.py -p ./s2n_demo/atg_configuration/s2n.py --verbose=True
```

#### Generate tests incrementally

```
./atg_scripts/atg_main.py -p ./s2n_demo/atg_configuration/s2n_demo.py --verbose=True
```

### 