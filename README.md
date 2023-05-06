# Electircity Dynamic peak/offpeak rates - Home Assistant [Energy Dashbaord]
Electricity On/Off Peak rates based on time of day usage for Energy Dashboard daily real-time calculations, create helpers to define the rate and on/off peak times.  Then a sensor that adjusts itself based on the time of day to parse peak/off peak rates to the grid consumption sensor within Home Assistant Energy Dashboard.

1. Create Helper "Input Number" called "ElectricityPeak", define Peak rate
```yaml
{
        "id": "electricitypeak",
        "min": 0.0,
        "max": 0.45,
        "unit_of_measurement": "AUD/kWh",
        "icon": "mdi:currency-usd",
        "mode": "box",
        "step": 0.00001,
        "name": "ElectricityPeak"
      }
```
![image](https://user-images.githubusercontent.com/84074944/236608884-a6109be4-ed5f-4b98-a961-050a1fa333ce.png)

![image](https://user-images.githubusercontent.com/84074944/236608913-2884610b-1ad3-4fdb-adff-bd228829ac1b.png)

2. Create Helper "Input Number" called "ElectricityOffPeak" define OffPeak rate
```yaml
      {
        "id": "electricityoffpeak",
        "min": 0.0,
        "max": 0.5,
        "name": "ElectricityOffpeak",
        "icon": "mdi:currency-usd-off",
        "step": 0.00001,
        "unit_of_measurement": "AUD/kWh",
        "mode": "box"
      }
```
![image](https://user-images.githubusercontent.com/84074944/236608943-374c542b-357e-4694-9443-583ea7c54e88.png)

![image](https://user-images.githubusercontent.com/84074944/236608958-4bbf608e-6a87-435a-898d-d0072d2a3860.png)

3. Create Helper "Times of Day Sensor" called "Peak" define the Peak time, ie 15:00:00 - 20:59:59 (3pm to 8.59pm)

![image](https://user-images.githubusercontent.com/84074944/236609018-403ee8cf-0c4c-4b33-a0b3-acebd01deb73.png)

![image](https://user-images.githubusercontent.com/84074944/236609035-ad79e420-b5c4-406f-83b2-c2026b46446d.png)

4. Create Helper "Times of Day Sensor" called "OffPeak" define the OffPeak time, ie 21:00:00 - 14:59:59 (9pm - 2.59pm)

![image](https://user-images.githubusercontent.com/84074944/236609047-484800d5-33dd-43ad-97b0-da5993031347.png)

![image](https://user-images.githubusercontent.com/84074944/236609057-a7a41929-800d-4953-af0f-c0752baa8d1f.png)

5. Enter code into configuration.yaml to create the "CurrentElectricityRate" to manage the rate per the time of day for defining the "Use an entity with current price" value in your grid consumption entry.
```yaml
template:
  - sensor:
      - name: "CurrentElectricityRate"
        unique_id: "6fc47be5-359f-4d4e-a4e1-f642d80d8020"
        unit_of_measurement: "AUD/kWh"
        state: >
          {% if is_state('binary_sensor.peak', 'on') %}
            {{ states("input_number.electricitypeak") }}
          {% else %}
            {{ states("input_number.electricityoffpeak") }}
          {% endif %}
```
6. Restart Home Assistant and then define the value in your grid consumption entity.

![image](https://user-images.githubusercontent.com/84074944/236609097-21870887-a222-4e19-bd61-b351fd63d967.png)
