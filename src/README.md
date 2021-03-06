# SDCWorks Simulator

The SDCWorks simulator can be run as follows:
```
python3 main.py [-options]
```

options are specified as follows:
* -h, --help,       prints the usage information
* -d, --directory   path of user-specified directory where files are located

For the program to run correctly, the user-specified directory will require a
plant.yaml and a requirements.yaml file. 
```
...user-specified directory/
    plant.yaml
    requirements.yaml
```

After the simulator has been run, it
will generate the following directories the user-specified directory:
* dot       graphs encoded in the DOT formatas parsed by the simulator of the
  graphs specified in the plant.yaml and requirement.yaml files
* graph     PNGs of the graphs from the 'dot' directory
* log       logs of all executions of the program. Each log contains a detailed
  state of the simulator at every timestep.
* metrics   contains all the data and plots associated with the metrics

# Input Files

Input to the SDCWorks simulator is specified in the plant.yaml and
requirements.yaml files in the user-specified directories. Examples of both
inputs can be found in the [examples](../examples/) directory. Below are the
specifications of the input files:

## Plant Input

```
cells:
    - source:
        name: CELL_NAME
        length: LENGTH

    - sink:
        name: CELL_NAME
        length: LENGTH

    - cell:
        name: CELL_NAME
        length: LENGTH
        operations:
            - [OP_NAME, OP_DURATION]
            
conveyors:
    - conveyor:
        length: LENGTH
        prev: [CELL_NAME]
        next: [CELL_NAME]

```

Variables:
* CELL_NAME   (type string): unique cell name 
* OP_NAME     (type string): name of operation
* OP_DURATION (type int): duration of operation
* LENGTH      (type integer): specifies the length of a cell's queue

A plant may only have one source and one sink at the moment. There can be
arbitrarily many cell/conveyor definitions as necessary to model a plant
floor. The "length" variable is optional for all cell/conveyor definitions and
will default to 1 if left unspecified. Each cell can have an arbritrary number
operations assigned to it.

## Requirements Input

```
requirements:
    - requirement:
        name: REQ_NAME
        nodes:
            - [REQ_NODE_NAME, OP_NAME]
        root: REQ_NODE_NAME
        edges:
            - [REQ_NODE_NAME, [REQ_NODE_NAME, ...]]
```

Variables:
* REQ_NAME        (type string): unique requirement name
* REQ_NODE_NAME   (type string): unique requirement node name
* OP_NAME         (type string): name of operation specified in plant.yaml

There can be multiple requirement definitions in a single file. A requirement
can have arbitrarily many nodes and edges but may only have a single root. To
reduce duplication, a requirement node can be connected to many other
requirement nodes in a single line. Note that edges are unidirectional.

# Controller

Currently, there is a baseline controller in the [custom](custom/) directory.
The simulator is very flexible and allows for the controller to be swapped out
with other implementations. In addition to the CustomController class, there are
CustomWidget and CustomCell classes. These classes allow for extra
functions/variables handling to support complex controllers where the
variables/functions in the respective generic classes may not provide enough
information. 
