.. _architecture_components:

===============
Core Components
===============

This section provides detailed documentation of ReservoirPy's core architectural
components, including class hierarchies, inheritance patterns, and component
responsibilities within the cognitive framework.

Node Hierarchy and Inheritance
===============================

The ReservoirPy architecture is built upon a clear inheritance hierarchy that
establishes the fundamental computational patterns:

.. mermaid::

   classDiagram
       class _Node {
           <<abstract>>
           +name: str
           +params: Dict
           +hypers: Dict
           +input_dim: Shape
           +output_dim: Shape
           +is_initialized: bool
           +is_trainable: bool
           +fitted: bool
           +initialize()
           +call()
           +state()
           +reset()
       }
       
       class Node {
           +forward: callable
           +backward: callable
           +partial_backward: callable
           +buffers: Dict
           +feedback: DistantFeedback
           +run(X)
           +fit(X, Y)
           +train(X, Y)
           +with_state()
           +with_feedback()
       }
       
       class Model {
           +nodes: List[Node]
           +edges: List[Tuple]
           +input_nodes: List[Node]
           +output_nodes: List[Node]
           +update_graph()
           +get_node()
           +data_dispatcher()
       }
       
       class Reservoir {
           +units: int
           +sr: float
           +lr: float
           +W: ndarray
           +Win: ndarray
           +bias: ndarray
           +activation: callable
       }
       
       class Ridge {
           +ridge: float
           +Wout: ndarray
           +bias: ndarray
           +input_bias: bool
       }
       
       class Input {
           +input_scaling: float
       }
       
       class Output {
           +return_states: bool
       }
       
       _Node <|-- Node
       Node <|-- Model
       Node <|-- Reservoir
       Node <|-- Ridge
       Node <|-- Input
       Node <|-- Output

**Base Abstraction Layer (_Node)**

The ``_Node`` abstract base class defines the fundamental interface for all
computational units in the ReservoirPy ecosystem:

- **Identity Management**: Unique naming and registry systems
- **Dimensionality**: Input/output shape specifications  
- **State Tracking**: Initialization and training status
- **Parameter Management**: Hyperparameters and learned parameters

**Computational Node Layer (Node)**

The ``Node`` class implements the core computational paradigm:

- **Forward Computation**: Data transformation through stateful operations
- **Backward Propagation**: Error signal propagation for learning
- **State Management**: Temporal state persistence and manipulation
- **Feedback Integration**: Distant node communication mechanisms

**Composition Layer (Model)**

The ``Model`` class enables hierarchical composition of computational graphs:

- **Graph Management**: Node connectivity and data flow routing
- **Execution Orchestration**: Coordinated multi-node computation
- **State Synchronization**: Distributed state management across nodes
- **Interface Abstraction**: Model-as-Node compositional patterns

Specialized Node Types
======================

ReservoirPy provides several specialized node implementations optimized for
different cognitive functions:

.. mermaid::

   graph TD
       subgraph "Reservoir Nodes"
           R1[Reservoir]
           ESN[ESN]
           NVAR[NVAR]
           IP[Intrinsic Plasticity]
       end
       
       subgraph "Readout Nodes"
           RR[Ridge Regression]
           W[Wout]
           O[Online Learning]
       end
       
       subgraph "Activation Nodes"
           T[Tanh]
           S[Sigmoid]
           R[ReLU]
           C[Custom]
       end
       
       subgraph "Utility Nodes"
           I[Input]
           OUT[Output]
           CON[Concat]
           D[Delay]
       end
       
       R1 --> ESN
       ESN --> NVAR
       R1 --> IP
       
       RR --> W
       W --> O
       
       Node --> R1
       Node --> RR
       Node --> T
       Node --> I

**Reservoir Cognitive Kernels**

Reservoir nodes implement the core cognitive processing units:

- **Reservoir**: Standard echo state network reservoir with configurable dynamics
- **ESN**: Complete echo state network with integrated reservoir and readout  
- **NVAR**: Nonlinear vector autoregression with reservoir dynamics
- **Intrinsic Plasticity**: Adaptive reservoir with self-organizing properties

**Readout Processing Units**

Readout nodes provide trainable output interfaces:

- **Ridge**: L2-regularized linear regression for stable learning
- **Wout**: Direct weight matrix readout for maximum flexibility
- **Online**: Incremental learning readouts for real-time adaptation

**Activation Functions**

Activation nodes provide non-linear transformation capabilities:

- **Standard Functions**: Tanh, Sigmoid, ReLU implementations
- **Custom Functions**: User-defined activation patterns
- **Adaptive Activations**: Learnable activation parameters

Component Interaction Patterns
===============================

The architecture defines several key interaction patterns that enable complex
cognitive behaviors:

.. mermaid::

   sequenceDiagram
       participant U as User
       participant M as Model
       participant N1 as Node1
       participant N2 as Node2
       participant S as State
       
       U->>M: call(data)
       activate M
       
       M->>N1: initialize(data)
       activate N1
       N1->>S: set_input_dim()
       N1->>S: create_state()
       N1-->>M: initialized
       deactivate N1
       
       M->>N1: call(data)
       activate N1
       N1->>S: update_state()
       N1-->>M: output1
       deactivate N1
       
       M->>N2: call(output1)
       activate N2
       N2->>S: update_state()
       N2-->>M: output2
       deactivate N2
       
       M-->>U: final_output
       deactivate M

**Forward Propagation Pattern**

The standard data flow pattern through connected nodes:

1. **Initialization Phase**: Dimension inference and parameter setup
2. **State Preparation**: Internal state vector allocation
3. **Forward Computation**: Sequential node execution with state updates
4. **Output Collection**: Result aggregation and return

**Feedback Loop Pattern**

Distant node communication for recurrent processing:

.. mermaid::

   stateDiagram-v2
       [*] --> Waiting
       Waiting --> Computing: input_received
       Computing --> StateUpdate: forward_pass
       StateUpdate --> FeedbackSend: state_ready
       FeedbackSend --> Waiting: feedback_transmitted
       
       StateUpdate --> FeedbackReceive: distant_feedback
       FeedbackReceive --> Computing: feedback_integrated

**Training Pattern**

Learning protocols for parameter adaptation:

- **Offline Training**: Batch learning with complete dataset
- **Online Training**: Incremental learning with streaming data
- **Partial Training**: Selective parameter updates
- **Unsupervised Learning**: Self-organizing parameter adaptation

State Management Architecture
=============================

ReservoirPy implements sophisticated state management for temporal processing:

.. mermaid::

   graph LR
       subgraph "State Types"
           IS[Internal State]
           PS[Proxy State]
           BS[Buffer State]
           FS[Feedback State]
       end
       
       subgraph "Operations"
           R[Reset]
           S[Save]
           L[Load]
           U[Update]
       end
       
       subgraph "Persistence"
           M[Memory]
           D[Disk]
           N[Network]
       end
       
       IS --> R
       PS --> S
       BS --> L
       FS --> U
       
       R --> M
       S --> D
       L --> N
       U --> M

**State Categories**

- **Internal State**: Primary computational state of each node
- **Proxy State**: Read-only state references for feedback connections
- **Buffer State**: Temporary storage for intermediate computations
- **Feedback State**: Cached state from distant nodes

**State Operations**

- **Context Management**: Temporary state modifications with automatic restoration
- **State Serialization**: Persistent storage and retrieval mechanisms  
- **State Synchronization**: Coordinated state updates across multiple nodes
- **State Validation**: Consistency checking and error detection

This architectural foundation enables ReservoirPy to support complex cognitive
computing patterns while maintaining computational efficiency and user-friendly
interfaces for reservoir computing applications.