New widget interface
====================

Constructor
-----------

`ExampleWidget(config, params)` (required)

* `config`: configuration information for the widget display screen for
  `example_widget`(object, required):
    * `container`: a unique id for this instance of the widget; also the id of
      the DOM object in which the widget must be displayed (string, required)
    * `static_url`: URL prefix under which the entire content of the widget
      source directory can be accessed (string, required)
* `params`: parameters for this instance of the widget (object, contents
  varies between widget classes and is opaque to the caller, required
  but could be empty)

Methods
-------

* `init()` (required)

  Initialise or-re-initialise the widget acording to `params` and render it in the
  DOM object identified by `config{container}`. This method may take
  responsibility for refreshing the widget in the future, otherwise there should
  be a `refresh()` method that does this (see below).

* `refresh()` (optional)

  Refresh the widget display.

* `configure(config, params)` (required)

  Render a configuration screen for the widget (displaying current
  parameters if there are any), collect and validate new parameters and
  return them.

    * `config`: configuration information for the configuration screen
      (object):
        * `widget_id`: unique id for this instance of the widget; also the id of
          the DOM object in which the widget display (not the widget
          configuration) is or will be displayed (string, required)
        * `config_id`: id of the DOM object in which the widget configuration
           should be displayed (string, required)
        * `configuration_callback`: a function callback to be called to
          return the new configuration information:
            * `function(config, config_params)`:
                * `config`: copy of the configuration information for the
                  configuration screen of caller (object, copy of `config` above)
                * `config_params`: the collected configuration parameters (object, contents
                  will vary and is opaque to the caller of the config method,
                  required). Will be an empty object if (re-)configuration
                  was cancelled in which case the configuration for this widget
                  must not be updated
    * `params`: current parameters of this instance of the widget (object, contents
    will vary and is opaque to the caller, an empty object when creating a new instance)

* `log(messang [message...])` (optional? required?)

Output message to logging stream (typically `console.log`)

* `do_load()` (optional)

Internal function used for code common to `init()` and `refresh()`

Other properties
----------------

* `container`: copy of the `config{container}` parameter passed to the
  constructor (read/write, though requires an immediate call to `init()`
  if changed)
* `params`: copy of the `params` parameter passed to the constructor
  (read/write, though requires an immediate call to `init()` if changed)

