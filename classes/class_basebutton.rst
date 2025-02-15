.. Generated automatically by doc/tools/makerst.py in Godot's source tree.
.. DO NOT EDIT THIS FILE, but the BaseButton.xml source instead.
.. The source is found in doc/classes or modules/<name>/doc_classes.

.. _class_BaseButton:

BaseButton
==========

**Inherits:** :ref:`Control<class_Control>` **<** :ref:`CanvasItem<class_CanvasItem>` **<** :ref:`Node<class_Node>` **<** :ref:`Object<class_Object>`

**Inherited By:** :ref:`Button<class_Button>`, :ref:`LinkButton<class_LinkButton>`, :ref:`TextureButton<class_TextureButton>`

**Category:** Core

Brief Description
-----------------

Base class for different kinds of buttons.

Properties
----------

+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`ActionMode<enum_BaseButton_ActionMode>` | :ref:`action_mode<class_BaseButton_property_action_mode>`                   | 1     |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`int<class_int>`                         | :ref:`button_mask<class_BaseButton_property_button_mask>`                   | 1     |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`bool<class_bool>`                       | :ref:`disabled<class_BaseButton_property_disabled>`                         | false |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`FocusMode<enum_Control_FocusMode>`      | :ref:`enabled_focus_mode<class_BaseButton_property_enabled_focus_mode>`     | 2     |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`ButtonGroup<class_ButtonGroup>`         | :ref:`group<class_BaseButton_property_group>`                               | null  |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`bool<class_bool>`                       | :ref:`keep_pressed_outside<class_BaseButton_property_keep_pressed_outside>` | false |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`bool<class_bool>`                       | :ref:`pressed<class_BaseButton_property_pressed>`                           | false |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`ShortCut<class_ShortCut>`               | :ref:`shortcut<class_BaseButton_property_shortcut>`                         | null  |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`bool<class_bool>`                       | :ref:`shortcut_in_tooltip<class_BaseButton_property_shortcut_in_tooltip>`   | true  |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+
| :ref:`bool<class_bool>`                       | :ref:`toggle_mode<class_BaseButton_property_toggle_mode>`                   | false |
+-----------------------------------------------+-----------------------------------------------------------------------------+-------+

Methods
-------

+-------------------------------------------+--------------------------------------------------------------------------------------------------------------+
| void                                      | :ref:`_pressed<class_BaseButton_method__pressed>` **(** **)** virtual                                        |
+-------------------------------------------+--------------------------------------------------------------------------------------------------------------+
| void                                      | :ref:`_toggled<class_BaseButton_method__toggled>` **(** :ref:`bool<class_bool>` button_pressed **)** virtual |
+-------------------------------------------+--------------------------------------------------------------------------------------------------------------+
| :ref:`DrawMode<enum_BaseButton_DrawMode>` | :ref:`get_draw_mode<class_BaseButton_method_get_draw_mode>` **(** **)** const                                |
+-------------------------------------------+--------------------------------------------------------------------------------------------------------------+
| :ref:`bool<class_bool>`                   | :ref:`is_hovered<class_BaseButton_method_is_hovered>` **(** **)** const                                      |
+-------------------------------------------+--------------------------------------------------------------------------------------------------------------+

Signals
-------

.. _class_BaseButton_signal_button_down:

- **button_down** **(** **)**

Emitted when the button starts being held down.

.. _class_BaseButton_signal_button_up:

- **button_up** **(** **)**

Emitted when the button stops being held down.

.. _class_BaseButton_signal_pressed:

- **pressed** **(** **)**

Emitted when the button is toggled or pressed. This is on :ref:`button_down<class_BaseButton_signal_button_down>` if :ref:`action_mode<class_BaseButton_property_action_mode>` is :ref:`ACTION_MODE_BUTTON_PRESS<class_BaseButton_constant_ACTION_MODE_BUTTON_PRESS>` and on :ref:`button_up<class_BaseButton_signal_button_up>` otherwise.

.. _class_BaseButton_signal_toggled:

- **toggled** **(** :ref:`bool<class_bool>` button_pressed **)**

Emitted when the button was just toggled between pressed and normal states (only if :ref:`toggle_mode<class_BaseButton_property_toggle_mode>` is active). The new state is contained in the ``button_pressed`` argument.

Enumerations
------------

.. _enum_BaseButton_DrawMode:

.. _class_BaseButton_constant_DRAW_NORMAL:

.. _class_BaseButton_constant_DRAW_PRESSED:

.. _class_BaseButton_constant_DRAW_HOVER:

.. _class_BaseButton_constant_DRAW_DISABLED:

.. _class_BaseButton_constant_DRAW_HOVER_PRESSED:

enum **DrawMode**:

- **DRAW_NORMAL** = **0** --- The normal state (i.e. not pressed, not hovered, not toggled and enabled) of buttons.

- **DRAW_PRESSED** = **1** --- The state of buttons are pressed.

- **DRAW_HOVER** = **2** --- The state of buttons are hovered.

- **DRAW_DISABLED** = **3** --- The state of buttons are disabled.

- **DRAW_HOVER_PRESSED** = **4**

.. _enum_BaseButton_ActionMode:

.. _class_BaseButton_constant_ACTION_MODE_BUTTON_PRESS:

.. _class_BaseButton_constant_ACTION_MODE_BUTTON_RELEASE:

enum **ActionMode**:

- **ACTION_MODE_BUTTON_PRESS** = **0** --- Require just a press to consider the button clicked.

- **ACTION_MODE_BUTTON_RELEASE** = **1** --- Require a press and a subsequent release before considering the button clicked.

Description
-----------

BaseButton is the abstract base class for buttons, so it shouldn't be used directly (it doesn't display anything). Other types of buttons inherit from it.

Property Descriptions
---------------------

.. _class_BaseButton_property_action_mode:

- :ref:`ActionMode<enum_BaseButton_ActionMode>` **action_mode**

+-----------+------------------------+
| *Default* | 1                      |
+-----------+------------------------+
| *Setter*  | set_action_mode(value) |
+-----------+------------------------+
| *Getter*  | get_action_mode()      |
+-----------+------------------------+

Determines when the button is considered clicked, one of the ``ACTION_MODE_*`` constants.

.. _class_BaseButton_property_button_mask:

- :ref:`int<class_int>` **button_mask**

+-----------+------------------------+
| *Default* | 1                      |
+-----------+------------------------+
| *Setter*  | set_button_mask(value) |
+-----------+------------------------+
| *Getter*  | get_button_mask()      |
+-----------+------------------------+

Binary mask to choose which mouse buttons this button will respond to.

To allow both left-click and right-click, use ``BUTTON_MASK_LEFT | BUTTON_MASK_RIGHT``.

.. _class_BaseButton_property_disabled:

- :ref:`bool<class_bool>` **disabled**

+-----------+---------------------+
| *Default* | false               |
+-----------+---------------------+
| *Setter*  | set_disabled(value) |
+-----------+---------------------+
| *Getter*  | is_disabled()       |
+-----------+---------------------+

If ``true``, the button is in disabled state and can't be clicked or toggled.

.. _class_BaseButton_property_enabled_focus_mode:

- :ref:`FocusMode<enum_Control_FocusMode>` **enabled_focus_mode**

+-----------+-------------------------------+
| *Default* | 2                             |
+-----------+-------------------------------+
| *Setter*  | set_enabled_focus_mode(value) |
+-----------+-------------------------------+
| *Getter*  | get_enabled_focus_mode()      |
+-----------+-------------------------------+

Focus access mode to use when switching between enabled/disabled (see :ref:`Control.focus_mode<class_Control_property_focus_mode>` and :ref:`disabled<class_BaseButton_property_disabled>`).

.. _class_BaseButton_property_group:

- :ref:`ButtonGroup<class_ButtonGroup>` **group**

+-----------+-------------------------+
| *Default* | null                    |
+-----------+-------------------------+
| *Setter*  | set_button_group(value) |
+-----------+-------------------------+
| *Getter*  | get_button_group()      |
+-----------+-------------------------+

:ref:`ButtonGroup<class_ButtonGroup>` associated to the button.

.. _class_BaseButton_property_keep_pressed_outside:

- :ref:`bool<class_bool>` **keep_pressed_outside**

+-----------+---------------------------------+
| *Default* | false                           |
+-----------+---------------------------------+
| *Setter*  | set_keep_pressed_outside(value) |
+-----------+---------------------------------+
| *Getter*  | is_keep_pressed_outside()       |
+-----------+---------------------------------+

If ``true``, the button stays pressed when moving the cursor outside the button while pressing it.

.. _class_BaseButton_property_pressed:

- :ref:`bool<class_bool>` **pressed**

+-----------+--------------------+
| *Default* | false              |
+-----------+--------------------+
| *Setter*  | set_pressed(value) |
+-----------+--------------------+
| *Getter*  | is_pressed()       |
+-----------+--------------------+

If ``true``, the button's state is pressed. Means the button is pressed down or toggled (if toggle_mode is active).

.. _class_BaseButton_property_shortcut:

- :ref:`ShortCut<class_ShortCut>` **shortcut**

+-----------+---------------------+
| *Default* | null                |
+-----------+---------------------+
| *Setter*  | set_shortcut(value) |
+-----------+---------------------+
| *Getter*  | get_shortcut()      |
+-----------+---------------------+

:ref:`ShortCut<class_ShortCut>` associated to the button.

.. _class_BaseButton_property_shortcut_in_tooltip:

- :ref:`bool<class_bool>` **shortcut_in_tooltip**

+-----------+----------------------------------+
| *Default* | true                             |
+-----------+----------------------------------+
| *Setter*  | set_shortcut_in_tooltip(value)   |
+-----------+----------------------------------+
| *Getter*  | is_shortcut_in_tooltip_enabled() |
+-----------+----------------------------------+

If ``true``, the button will add information about its shortcut in the tooltip.

.. _class_BaseButton_property_toggle_mode:

- :ref:`bool<class_bool>` **toggle_mode**

+-----------+------------------------+
| *Default* | false                  |
+-----------+------------------------+
| *Setter*  | set_toggle_mode(value) |
+-----------+------------------------+
| *Getter*  | is_toggle_mode()       |
+-----------+------------------------+

If ``true``, the button is in toggle mode. Makes the button flip state between pressed and unpressed each time its area is clicked.

Method Descriptions
-------------------

.. _class_BaseButton_method__pressed:

- void **_pressed** **(** **)** virtual

Called when the button is pressed.

.. _class_BaseButton_method__toggled:

- void **_toggled** **(** :ref:`bool<class_bool>` button_pressed **)** virtual

Called when the button is toggled (only if toggle_mode is active).

.. _class_BaseButton_method_get_draw_mode:

- :ref:`DrawMode<enum_BaseButton_DrawMode>` **get_draw_mode** **(** **)** const

Returns the visual state used to draw the button. This is useful mainly when implementing your own draw code by either overriding _draw() or connecting to "draw" signal. The visual state of the button is defined by the ``DRAW_*`` enum.

.. _class_BaseButton_method_is_hovered:

- :ref:`bool<class_bool>` **is_hovered** **(** **)** const

Returns ``true`` if the mouse has entered the button and has not left it yet.

