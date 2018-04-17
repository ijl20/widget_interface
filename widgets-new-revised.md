Proposed better new widget interface
====================================

_Version 3_

`params_object`
---------------

Some methods recieve or return a `params_object`. This is an object supporting JSON seralisation representing the configuration of a particular widget instance but its structure and content is otherwise opaque to the method's caller.
 
Constructor
-----------

`ExampleWidget(widget_id)` (required)

* `widget_id`: a unique id for this instance of the widget (string, required)

Methods
-------

* `display(config, params)` (required)

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

  * `config`: configuration information for the widget
    (object, required):
      * `container_id`: the id of the DOM object in which the
        widget must be displayed. [string, required, commonly
        having the same value as `widget_id`]
      * `static_url`: URL prefix under which the entire
        content of the widget source directory can be accessed.
        [string, required]
  * `params`: `params_object` containing the configuration for 
    this widget instance. [required]
   
  This method has no return value.

* `refresh()` (optional)

  Refresh the widget display.

  This method has no return value.

* `obj = configure(config, current_params)` (required)

  Render a configuration screen for the widget (initialised
  with current parameters if there are any), collect and validate
  new parameters and return them.

  ### Parameters

    * `config`: configuration information for the configuration
      screen (object, required):
        * `config_id`: id of the DOM object in which the widget
          configuration should be displayed. [string, required,
          commonly `<widget_id>_config`]
        * `static_url`: URL prefix under which the entire
          content of the widget source directory can be accessed.
          [string, required]
    * `current_params`: `params_object` containing the
      current configuration for this widget instance. [required, 
      pass an empty object when creating a new configuration]

  ### Return value

  An object containing the following keys:

   * `valid`: a parameterless function which when called 
      will trigger validation of the curren values and return 
      `true` if they were valid and `false` if not. If the 
      function returns `false` then the widget is responsible 
      for alerting the user to the problems. [boolean,
      required]
  * `value`: a parameterless function which when called returns
    a `params_object` containing a new or updated 
    configuration. [required]

Other properties
----------------

Explicitly none -- these obects have no other externally-accessible properties.
