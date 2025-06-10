.. _architecture:

====================
Architecture Guide
====================

{{ header }}

This guide provides comprehensive documentation of ReservoirPy's architecture,
including detailed system diagrams and component interaction patterns.

Understanding ReservoirPy's cognitive architecture is essential for building
effective reservoir computing models and extending the framework.

.. toctree::
    :maxdepth: 2
    :hidden:
    :titlesonly:

    overview
    components
    dataflow
    execution

.. grid:: 1 2 2 2

    .. grid-item-card::
        :class-card: intro-card
        :shadow: md

        System Overview
        ^^^^^^^^^^^^^^^

        High-level architecture overview showing the main cognitive subsystems
        and their emergent relationships.

        +++

        .. button-ref:: architecture_overview
            :color: secondary
            :click-parent:
            :expand:

            System Overview

    .. grid-item-card::
        :class-card: intro-card
        :shadow: md

        Core Components
        ^^^^^^^^^^^^^^^

        Detailed breakdown of architectural components including Nodes, Models,
        and their hierarchical relationships.

        +++

        .. button-ref:: architecture_components
            :color: secondary
            :click-parent:
            :expand:

            Core Components

    .. grid-item-card::
        :class-card: intro-card
        :shadow: md

        Data Flow Patterns
        ^^^^^^^^^^^^^^^^^^

        Data and signal propagation pathways through the cognitive architecture,
        including feedback loops and state management.

        +++

        .. button-ref:: architecture_dataflow
            :color: secondary
            :click-parent:
            :expand:

            Data Flow Patterns

    .. grid-item-card::
        :class-card: intro-card
        :shadow: md

        Execution Sequences
        ^^^^^^^^^^^^^^^^^^^

        Training and inference execution patterns showing temporal dynamics
        and adaptive attention allocation mechanisms.

        +++

        .. button-ref:: architecture_execution
            :color: secondary
            :click-parent:
            :expand:

            Execution Sequences