Proposed better new widget interface
====================================

_Version 5_

Data structures
---------------

### `params_object`

Some methods receive or return a `params_object`. This is an object
supporting JSON serialisation representing the parameters of a
particular instance of a particular widget. Its structure and content
is opaque to the method's caller.

### `config_object`

Some methods receive a `config_object`. This is an object containing
generic configuration information supplied by the method's caller. It
contains the following keys:

* `container_id`: the id of the DOM object within which
  the widget or it's configuration page must be displayed.
  [string, required, commonly based on `widget_id`]
* `static_url`: URL prefix via which the content of the widget
  source directory can be accessed. [string, required]
* `height`: the height in pixels of the area that the widget occupies
  in the current layout [integer, optional]
* `width`: the width in pixels of the area that the widget occupies
  in the current layout [integer, optional]
* `settings`: an object containing a copy of configuration parameters
  inherited from the enclosing framework's setup. Only parameters with
  names starting `SMARTPANEL_` are included [object, required]. Currently
  use values:
      * `SMARTPANEL_TRANSPORT_API`: URL base for the TFC transport API
      * `SMARTPANEL_RT_API`: URL base for the real time bus traffic data

Constructor
-----------

`ExampleWidget(widget_id)` (required)

* `widget_id`: a unique id for this instance of the widget (string, required)

Methods
-------

* `display(config_object, params_object)` (required)

  Display or re-display the widget. This method may take
  responsibility for refreshing the widget in the future,
  otherwise there should be a `refresh()` method that
  does this (see below).

  This method may be called more than once on any particular
  widget instance. When called a second and subsequent time the
  method must re-initialise data structures,
  cancel running timers, unsubscribe from external
  subscriptions, etc.

  ### Parameters:

  * `config_object`: the display configuration for the widget
    [`config_object`, required].
  * `params_object`: the parameters for this widget instance.
    [`params_object`, required]

  This method has no return value.

* `refresh()` (optional)

  Refresh the widget display.

  This method has no return value.

* `obj = configure(config_object, current_params_object)` (required)

  Render a configuration screen for the widget (initialised
  with current parameters if there are any), collect and validate
  new parameters and return them.

  ### Parameters

  * `config_object`: the display configuration for the widget
    [`config_object`, required].
  * `current_params_object`: the current parameters for the widget instance
    being edited. [`params_object`, required]

  ### Return value

  An object containing the following keys:

  * `valid`: a parameterless function which when called
     will trigger validation of the current values and return
     `true` if they were valid and `false` if not. If the
     function returns `false` then the widget should alert
     the user to the problems. [boolean,
     required]
  * `value`: a parameterless function which when called returns
     a `params_object` containing the new or updated
     configuration. [required]
  * `config`: an object containing the following keys [object, required]:
      * `title`: a short text description of this configuration [required]

Other properties
----------------

Explicitly none -- these obects have no other externally-accessible properties.
