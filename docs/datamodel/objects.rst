.. _ref_datamodel_object_types:

============
Object types
============


*Object types* are the primary components of EdgeDB schema. They are analogous
to SQL *tables* or ORM *models*, and consist of :ref:`properties
<ref_datamodel_props>` and :ref:`links <ref_datamodel_links>`.

.. Properties
.. ----------

Properties are used to attach primitive data to an object type. They are
declared with the ``property`` keyword. For the full documentation on
properties, see :ref:`Properties <ref_datamodel_props>`.

.. code-block:: sdl

  type Person {
    property email -> str;
  }

Links are used to define relationships between object types. They are declared
with the ``link`` keyword. For the full documentation on links, see :ref:`Links
<ref_datamodel_links>`.

.. code-block:: sdl

  type Person {
    link best_friend -> Person;
  }


IDs
---

There's no need to manually declare a primary key on your object types. All
object types automatically contain a property ``id`` of type ``UUID`` that's
*required*, *globally unique*, and *readonly*. This ``id`` is assigned upon
creation and never changes.

Abstract types
--------------

Object types can either be *abstract* or *non-abstract*. By default all object
types are non-abstract. You can't create or store instances of abstract types,
but they're a useful way to share functionality and structure across among
other object types.

.. code-block:: sdl

  abstract type HasName {
    property first_name -> str;
    property last_name -> str;
  }

Abstract types are commonly used in tandem with inheritance.

.. _ref_datamodel_objects_inheritance:

Inheritance
-----------

Object types can *extend* other object types. The extending type (AKA the
*subtype*) inherits all links, properties, indexes, constraints, etc. from its
*supertypes*.

.. code-block:: sdl

  abstract type Animal {
    property species -> str;
  }

  type Dog extending Animal {
    property breed -> str;
  }

.. _ref_datamodel_objects_multiple_inheritance:

Multiple Inheritance
^^^^^^^^^^^^^^^^^^^^

Object types can :ref:`extend more
than one type <ref_eql_sdl_object_types_inheritance>` — that's called
*multiple inheritance*. This mechanism allows building complex object
types out of combinations of more basic types.

.. code-block:: sdl

  abstract type HasName {
    property first_name -> str;
    property last_name -> str;
  }

  abstract type Email {
    property email -> str;
  }

  type Person extending HasName, HasEmail {
    property profession -> str;
  }

Name conflicts are not allowed; supertypes cannot share any link or property
names.


.. note::

  Refer to the dedicated pages on :ref:`Indexes <ref_datamodel_indexes>`,
  :ref:`Constraints <ref_datamodel_constraints>`, and :ref:`Annotations
  <ref_datamodel_annotations>` for full documentation on those concepts.

.. list-table::

  * - **See also**
  * - :ref:`SDL > Object types <ref_eql_sdl_object_types>`
  * - :ref:`DDL > Object types <ref_eql_ddl_object_types>`
  * - :ref:`Introspection > Object types <ref_eql_introspection_object_types>`
  * - :ref:`Cheatsheets > Object types <ref_cheatsheet_object_types>`
