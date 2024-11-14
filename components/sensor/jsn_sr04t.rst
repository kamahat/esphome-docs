JSN-SR04T Waterproof Ultrasonic Range Finder
============================================

.. seo::
    :description: Instructions for setting up JSN-SR04T waterproof ultrasonic distance sensor in ESPHome.
    :image: jsn-sr04t-v3.jpg
    :keywords: JSN-SR04T

This sensor allows you to use the JSN-SR04T and AJ_SR04M Ultrasonic Range Finder **in Mode 1 and 2**
with ESPHome to measure distances.

JSN-SR04T (v3.0) can measure ranges between 21 centimeters and 600 centimeters with a resolution of 1 millimeter.

AJ-SR04M can measure ranges between 20 centimeters and 800 centimeters with a resolution of 1 millimeter. It has a waterproof sensor and can be supplied in 3V3.

Configure the JSN-SR04T for mode 1:
    - **V1.0 and V2.0**: Add a 47k resistor to pad R27.
    - **V3.0**: Short pad M1 or add 47k resistor to pad mode.

Configure the JSN-SR04T for mode 2:
    - **V1.0 and V2.0**: Add a 120k resistor to pad R27.
    - **V3.0**: Short pad M2 or add 120k resistor to pad mode.

the AJ_SR04M as 5 modes : 

Mode 0: Compatible market HR-04 trigger mode Common - Pulse Width Square Wave (Minimum Power Consumption 2.5mA)
    - Open Circuit
Mode 1: Low Power mode - Pulse Width Square Wave (Minimum Power Consumption 40uA)
    - Add a 300k resistor to pad R19.
Mode 2: Automatic **Serial Port** mode - (Minimum Power Consumption 2.5mA)
    - Add a 120k resistor to pad R19.
Mode 3: Lower power **Serial Port** Trigger (Minimum Power Consumption 20uA)
    - Add a 47k resistor to pad R19.
Mode 4: ASCII Code output (Minimum Power Consumption 20uA)
    - Add a 0k resistor to pad R19.


.. figure:: images/jsn-sr04t-v3-mode-select-pads.jpg
    :align: center
    :width: 50.0%

    JSN-SR04T Waterproof Ultrasonic Range Finder Mode Select Pads.

In mode 1 the module continuously takes measurements approximately every 100mS and outputs the distance on the TX pin at 9600 baud.
In this mode :ref:`sensor-filters` are highly recommended.

In mode 2 the module takes a measurement only when a trigger command of 0x55 is sent to the RX pin on the module.
The module then outputs the distance on its TX pin. The frequency of the measurements can be set with the **update_interval** option.

To use the sensor, first set up an :ref:`uart` with a baud rate of 9600 and connect the sensor to the specified pin.

.. figure:: images/jsn-sr04t-v3.jpg
    :align: center
    :width: 70.0%

    JSN-SR04T Waterproof Ultrasonic Range Finder.

.. code-block:: yaml

    # Example configuration entry
    substitutions:
        #Serial mode : 
        #ultrasonic_tx_pin: GPIO01 # Serial TX
        #ultrasonic_rx_pin: GPIO03 # Serial RX
        ultrasonic_tx_pin: GPIO33 # Standard HC-SR04 (mode 1) trigger
        ultrasonic_rx_pin: GPIO32 # Standard HC-SR04 (mode 1) RX

    sensor:
      - platform: ultrasonic
        name: "Distance"
        timeout: 8m
        update_interval: 2s
        echo_pin: ${ultrasonic_rx_pin} 
        trigger_pin: 
          number: ${ultrasonic_tx_pin}
        echo_pin: 
          number: ${ultrasonic_rx_pin}
        unit_of_measurement: cm
        pulse_time: 20us    #value for AJ-SR04M 
        accuracy_decimals: 1
        filters:
          - filter_out: nan
          - multiply: 100
          - clamp:
              min_value: 25
              max_value: 800
              ignore_out_of_range: true

Configuration variables:
------------------------

- **update_interval** (*Optional*, :ref:`config-time`): The interval to check the
  sensor. Defaults to ``60s``. Not applicable for JSN-SR04T in mode 1.
- All other options from :ref:`Sensor <config-sensor>`.

See Also
--------

- :ref:`uart`
- :ref:`sensor-filters`
- :apiref:`jsn_sr04t/jsn_sr04t.h`
- :ghedit:`Edit`
