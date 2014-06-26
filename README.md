Analog Hardwarepins Library
===========================

This library provides a basic interface to the Analog input pins on the Kinoma Create, or other devices supporting the Kinoma Hardwarepins API.

Getting Started
---------------

This library can be imported into Kinoma Studio and added to an application's build path settings. You can then use the Analog module to create objects that operate on analog input pins on the hardware.

The following objects can be created using the Analog module in this library:

* [Input](#input)

Example
-------

The following example demonstrates how to instantiate a new Input object and poll the analog value from the sensor.

```xml
<program xmlns="http://www.kinoma.com/kpr/1">
    <require id="Analog" path="Analog" />
    <script>
        <![CDATA[
            // create a new Input object to poll for changes on the
            // analog input pin 51
            var input = new Analog.Input( 51 );
            
            // initialize the input pin and start polling
            input.init();
            input.poll( 50, "milliseconds", "/analog/value" );       
        ]]>
    </script>
    <handler path="/analog/value">
        <behavior>
            <method id="onInvoke" params="handler, message">
                <![CDATA[
                    var data = message.requestObject;

                    if( data != null )
                        trace( data.value + "\n" );
                ]]>
            </method>
        </behavior>
    </handler>
</program>
```

API Reference
-------------

The following reference describes the interfaces defined in the Analog module.

Input
------

The Input object interface is used to listen for changes to an analog input pin on the hardware device. The interface includes methods for getting the current state of the pin, and for polling the pin over time. When polling an analog input pin, it will invoke a callback handler that you specify when the pin state changes.

Creating a new instance of an Input object:

```javascript
var input = new Analog.Input( pin, config );
```

Initializing the Input object instance:

```javascript
input.init();
```

To request the current state of the input pin, you must specify a callback to receive the result. The callback is the name of a handler that is implemented in your program or module:

```javascript
input.get( callback );
```

To poll for a state change of the input pin, you must specify the polling interval and time units, as well as the callback handler to receive the change event:

```javascript
input.poll( time, units, callback, skipFirst );
```

The units parameter must be one of the following string literal values:

* milliseconds
* seconds
* minutes
* microseconds
* nanoseconds
* hours
* days

When the Input object's callback handler is invoked, the result will be passed in the message requestObject property. The structure of the result object will be as follows:

```javascript
{ value:.472 }
```