# ESPHome-Tuvio-ACI07W11
ESPHome template for Tuvio ACI07W11 WiFi Smart AC

What's needed:

* JST PH 2.0мм 4pin both sides with wires
* ESP32-C3 SuperMini
* SMD 0603 23.2 Om

No extra bullshit, here is working teamplate:

```yaml
substitutions:
  name: esphome-air-bedroom
  friendly_name: ESPHome Air Bedroom
  devicename: bedroom_ac
  upper_devicename: Bedroom AC

esphome:
  compile_process_limit: 2
  name: ${name}
  friendly_name: ${friendly_name}
  name_add_mac_suffix: false
  project:
    name: esphome.web
    version: '1.0'
  
esp32:
  board: esp32-c3-devkitm-1
  variant: esp32c3
  framework:
    type: esp-idf
    #type: arduino

logger:
    level: DEBUG
    hardware_uart: UART1
    baud_rate: 0

esp32_ble:
  enable_on_boot: false

# Enable Home Assistant API
api:

# Allow Over-The-Air updates
ota:
  platform: esphome

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  # Set up a wifi access point
  ap: {}

# In combination with the `ap` this allows the user
# to provision wifi credentials to the device via WiFi AP.
captive_portal:

dashboard_import:
  package_import_url: github://esphome/firmware/esphome-web/esp32c3.yaml@v2
  import_full_config: true

# Sets up Bluetooth LE (Only on ESP32) to allow the user
# to provision wifi credentials to the device.
esp32_improv:
  authorizer: none

# To have a "next url" for improv serial
web_server:

uart:
  tx_pin:
    number: 1
    inverted: false
  rx_pin:
    number: 3
    inverted: false
    mode: input
  baud_rate: 9600
  debug:

tuya:
  
climate:
  - platform: tuya
    name: Bedroom AC
    id: iqool
    supports_heat: true
    supports_cool: true
    switch_datapoint: 1
    target_temperature_datapoint: 2
    current_temperature_datapoint: 3
    active_state:
      datapoint: 4
      cooling_value: 1
      fanonly_value: 4
      drying_value: 3
      heating_value: 2
    fan_mode:
      datapoint: 5
      auto_value: 0
      #high_value: 1
      low_value: 2
      medium_value: 3
      high_value: 4
      #middle_value: 4
    #preset:
    #  sleep:
    #    datapoint: 101
    #  eco:
    #    datapoint: 8
    swing_mode:
      vertical_datapoint: 30
      horizontal_datapoint: 33
    visual: # Optional. Example of visual settings override.
      min_temperature: 16 °C
      max_temperature: 30 °C
      temperature_step: 1 °C

switch:
  - platform: tuya
    id: contact_fixer
    switch_datapoint: 101

sensor:
  - platform: "tuya"
    name: "AC Measured Temperature"
    sensor_datapoint: 3
  - platform: "tuya"
    name: "AC Setpoint"
    sensor_datapoint: 2
  - platform: "tuya"
    name: "AC Mode"
    sensor_datapoint: 4
    accuracy_decimals: 0
  - platform: "tuya"
    name: "AC Fan Speed"
    sensor_datapoint: 5
    accuracy_decimals: 0

binary_sensor:
  - platform: "tuya"
    name: "AC On"
    sensor_datapoint: 1
```
