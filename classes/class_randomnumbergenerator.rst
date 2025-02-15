.. Generated automatically by doc/tools/makerst.py in Godot's source tree.
.. DO NOT EDIT THIS FILE, but the RandomNumberGenerator.xml source instead.
.. The source is found in doc/classes or modules/<name>/doc_classes.

.. _class_RandomNumberGenerator:

RandomNumberGenerator
=====================

**Inherits:** :ref:`Reference<class_Reference>` **<** :ref:`Object<class_Object>`

**Category:** Core

Brief Description
-----------------

A class for generating pseudo-random numbers.

Properties
----------

+-----------------------+--------------------------------------------------------+----------------------+
| :ref:`int<class_int>` | :ref:`seed<class_RandomNumberGenerator_property_seed>` | -6398989897141750821 |
+-----------------------+--------------------------------------------------------+----------------------+

Methods
-------

+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| :ref:`float<class_float>` | :ref:`randf<class_RandomNumberGenerator_method_randf>` **(** **)**                                                                               |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| :ref:`float<class_float>` | :ref:`randf_range<class_RandomNumberGenerator_method_randf_range>` **(** :ref:`float<class_float>` from, :ref:`float<class_float>` to **)**      |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| :ref:`float<class_float>` | :ref:`randfn<class_RandomNumberGenerator_method_randfn>` **(** :ref:`float<class_float>` mean=0.0, :ref:`float<class_float>` deviation=1.0 **)** |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| :ref:`int<class_int>`     | :ref:`randi<class_RandomNumberGenerator_method_randi>` **(** **)**                                                                               |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| :ref:`int<class_int>`     | :ref:`randi_range<class_RandomNumberGenerator_method_randi_range>` **(** :ref:`int<class_int>` from, :ref:`int<class_int>` to **)**              |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| void                      | :ref:`randomize<class_RandomNumberGenerator_method_randomize>` **(** **)**                                                                       |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Description
-----------

RandomNumberGenerator is a class for generating pseudo-random numbers. It currently uses `PCG32 <http://www.pcg-random.org/>`_.

**Note:** The underlying algorithm is an implementation detail. As a result, it should not be depended upon for reproducible random streams across Godot versions.

To generate a random float number (within a given range) based on a time-dependant seed:

::

    var rng = RandomNumberGenerator.new()
    func _ready():
        rng.randomize()
        var my_random_number = rng.randf_range(-10.0, 10.0)

Property Descriptions
---------------------

.. _class_RandomNumberGenerator_property_seed:

- :ref:`int<class_int>` **seed**

+-----------+----------------------+
| *Default* | -6398989897141750821 |
+-----------+----------------------+
| *Setter*  | set_seed(value)      |
+-----------+----------------------+
| *Getter*  | get_seed()           |
+-----------+----------------------+

The seed used by the random number generator. A given seed will give a reproducible sequence of pseudo-random numbers.

**Note:** The RNG does not have an avalanche effect, and can output similar random streams given similar seeds. Consider using a hash function to improve your seed quality if they're sourced externally.

Method Descriptions
-------------------

.. _class_RandomNumberGenerator_method_randf:

- :ref:`float<class_float>` **randf** **(** **)**

Generates a pseudo-random float between ``0.0`` and ``1.0`` (inclusive).

.. _class_RandomNumberGenerator_method_randf_range:

- :ref:`float<class_float>` **randf_range** **(** :ref:`float<class_float>` from, :ref:`float<class_float>` to **)**

Generates a pseudo-random float between ``from`` and ``to`` (inclusive).

.. _class_RandomNumberGenerator_method_randfn:

- :ref:`float<class_float>` **randfn** **(** :ref:`float<class_float>` mean=0.0, :ref:`float<class_float>` deviation=1.0 **)**

Generates a `normally-distributed <https://en.wikipedia.org/wiki/Normal_distribution>`_ pseudo-random number, using Box-Muller transform with the specified ``mean`` and a standard ``deviation``. This is also called Gaussian distribution.

.. _class_RandomNumberGenerator_method_randi:

- :ref:`int<class_int>` **randi** **(** **)**

Generates a pseudo-random 32-bit unsigned integer between ``0`` and ``4294967295`` (inclusive).

.. _class_RandomNumberGenerator_method_randi_range:

- :ref:`int<class_int>` **randi_range** **(** :ref:`int<class_int>` from, :ref:`int<class_int>` to **)**

Generates a pseudo-random 32-bit signed integer between ``from`` and ``to`` (inclusive).

.. _class_RandomNumberGenerator_method_randomize:

- void **randomize** **(** **)**

Setups a time-based seed to generator.

