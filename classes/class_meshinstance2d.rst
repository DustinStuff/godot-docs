.. Generated automatically by doc/tools/makerst.py in Godot's source tree.
.. DO NOT EDIT THIS FILE, but the MeshInstance2D.xml source instead.
.. The source is found in doc/classes or modules/<name>/doc_classes.

.. _class_MeshInstance2D:

MeshInstance2D
==============

**Inherits:** :ref:`Node2D<class_Node2D>` **<** :ref:`CanvasItem<class_CanvasItem>` **<** :ref:`Node<class_Node>` **<** :ref:`Object<class_Object>`

**Category:** Core

Brief Description
-----------------

Node used for displaying a :ref:`Mesh<class_Mesh>` in 2D.

Properties
----------

+-------------------------------+-------------------------------------------------------------+------+
| :ref:`Mesh<class_Mesh>`       | :ref:`mesh<class_MeshInstance2D_property_mesh>`             | null |
+-------------------------------+-------------------------------------------------------------+------+
| :ref:`Texture<class_Texture>` | :ref:`normal_map<class_MeshInstance2D_property_normal_map>` | null |
+-------------------------------+-------------------------------------------------------------+------+
| :ref:`Texture<class_Texture>` | :ref:`texture<class_MeshInstance2D_property_texture>`       | null |
+-------------------------------+-------------------------------------------------------------+------+

Signals
-------

.. _class_MeshInstance2D_signal_texture_changed:

- **texture_changed** **(** **)**

Description
-----------

Node used for displaying a :ref:`Mesh<class_Mesh>` in 2D. Can be constructed from an existing :ref:`Sprite<class_Sprite>` use tool in Toolbar. Select "Sprite" then "Convert to Mesh2D", select settings in popup and press "Create Mesh2D".

Tutorials
---------

- :doc:`../tutorials/2d/2d_meshes`

Property Descriptions
---------------------

.. _class_MeshInstance2D_property_mesh:

- :ref:`Mesh<class_Mesh>` **mesh**

+-----------+-----------------+
| *Default* | null            |
+-----------+-----------------+
| *Setter*  | set_mesh(value) |
+-----------+-----------------+
| *Getter*  | get_mesh()      |
+-----------+-----------------+

The :ref:`Mesh<class_Mesh>` that will be drawn by the ``MeshInstance2D``.

.. _class_MeshInstance2D_property_normal_map:

- :ref:`Texture<class_Texture>` **normal_map**

+-----------+-----------------------+
| *Default* | null                  |
+-----------+-----------------------+
| *Setter*  | set_normal_map(value) |
+-----------+-----------------------+
| *Getter*  | get_normal_map()      |
+-----------+-----------------------+

The normal map that will be used if using the default :ref:`CanvasItemMaterial<class_CanvasItemMaterial>`.

.. _class_MeshInstance2D_property_texture:

- :ref:`Texture<class_Texture>` **texture**

+-----------+--------------------+
| *Default* | null               |
+-----------+--------------------+
| *Setter*  | set_texture(value) |
+-----------+--------------------+
| *Getter*  | get_texture()      |
+-----------+--------------------+

The :ref:`Texture<class_Texture>` that will be used if using the default :ref:`CanvasItemMaterial<class_CanvasItemMaterial>`. Can be accessed as ``TEXTURE`` in CanvasItem shader.

