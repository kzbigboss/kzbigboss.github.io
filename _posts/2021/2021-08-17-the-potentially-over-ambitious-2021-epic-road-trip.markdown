---
layout: "post"
title: "The (potentially over-ambitious) 2021 Epic Road Trip"
date: "2021-08-17 11:10"
featured_image: images/2021/08/2021epicroadtrip.png
category: drives
---

Do the best laid plans lay wasted?  I am going to find out.  Read on to learn about my plans for my next road trip and the data I will be capturing along the way.

## The Drive

For my next road trip, I planned a cross-country route that is 8,726 miles long and will take 5 days, 10 hours, and 19 minutes to drive.  That route, as well as some potential donut shops to visit, looks like:

<div class="map-responsive">
<iframe src="https://www.google.com/maps/d/embed?mid=1uRlDMaQQDUU7Q-OxdIDoyR-wwiioHIaC&hl=en" width="640" height="480"></iframe>
</div>

The motivation for this trip is two-fold:
1. I want to visit my brother's new house out near Boston.
2. I want to take my Tesla Model Y out on a long road trip.  

I originally envisioned this as an opportunity to bounce between Seattle and Boston.  Being on sabbatical is giving me an opportunity to expand on that.  Will I hit every city and donut shop on this map?  Probably not.  I really enjoy road trips but I am also a pragmatist: I will no doubt get physically and/or mentally tired of driving.

## The Data

In my first road trip in a Tesla ([see my GitHub write-up about it](https://github.com/kzbigboss/kzteslaroadtrip201801/blob/master/kzteslaroadtrip.md)), I stood up an AWS EC2 instance, pinged an unofficial Tesla API once a minute, and saved vehicle state data to an RDS instance.  Since then I started using [TeslaFi](https://www.teslafi.com/) to handle capturing and persisting the same data.  They have an API available that presents both the raw data captured via Tesla's APIs (e.g.: GPS, shift state, indoor/outdoor temperature) as well as some derived data (e.g.: drive number, sleep timer, GPS elevation, etc.).  You can check out what a good payload looks like [below](#example-teslafi-api-response-fully-populated).

This dataset is not as great as I remembered: it is somewhat sparse.  For my 55 minute drive between Seattle, WA and Lynwood, WA - it captured 71 data points with 67 unique GPS coordinates.  That fidelity felt good until I drew a map of the resultant GPS coordinates:

Example:
![2021-08-17-seattle-to-lynnwood-drive](/images/2021/08/2021-08-17-seattle-to-lynnwood-drive.png)

Turns out some minutes TeslaFi is providing more than one data point (as high as three) and other times it takes 2-3 minutes for a new observation.  This will be fine given the distances I will be making, though.


---


### Example TeslaFI API Response (fully populated)
```json
{
  "data_id": 1,
  "Date": "2021-08-17 11:48:32",
  "calendar_enabled": "1",
  "remote_start_enabled": "",
  "vehicle_id": null,
  "display_name": "Nebula++",
  "color": null,
  "fast_charger_brand": "",
  "notifications_enabled": "",
  "vin": null,
  "conn_charge_cable": "",
  "id": null,
  "charge_port_cold_weather_mode": "0",
  "id_s": null,
  "state": "online",
  "option_codes": "AD15,MDL3,PBSB,RENA,BT37,ID3W,RF3G,S3PB,DRLH,DV2W,",
  "user_charge_enable_request": null,
  "time_to_full_charge": "0.0",
  "charge_current_request": "48",
  "charge_enable_request": "1",
  "charge_to_max_range": "0",
  "charger_phases": null,
  "battery_heater_on": "0",
  "managed_charging_start_time": null,
  "battery_range": "213.06",
  "charger_power": "0",
  "charge_limit_soc": "90",
  "charger_pilot_current": "48",
  "charge_port_latch": "Engaged",
  "battery_current": "",
  "charger_actual_current": "0",
  "scheduled_charging_pending": "0",
  "fast_charger_type": "",
  "usable_battery_level": "68",
  "motorized_charge_port": "1",
  "charge_limit_soc_std": "90",
  "not_enough_power_to_heat": null,
  "battery_level": "68",
  "charge_energy_added": "1.15",
  "charge_port_door_open": "0",
  "max_range_charge_counter": "0",
  "charge_limit_soc_max": "100",
  "ideal_battery_range": "213.06",
  "managed_charging_active": "0",
  "charging_state": "Disconnected",
  "fast_charger_present": "0",
  "trip_charging": "0",
  "managed_charging_user_canceled": "0",
  "scheduled_charging_start_time": null,
  "est_battery_range": "204.04",
  "charge_rate": "0.0",
  "charger_voltage": "2",
  "charge_current_request_max": "48",
  "eu_vehicle": "0",
  "charge_miles_added_ideal": "5.0",
  "charge_limit_soc_min": "50",
  "charge_miles_added_rated": "5.0",
  "inside_temp": "21.5",
  "longitude": "-122.250605",
  "heading": "182",
  "gps_as_of": "1629226108",
  "latitude": "47.760194",
  "speed": null,
  "shift_state": null,
  "seat_heater_rear_right": "0",
  "seat_heater_rear_left_back": "",
  "seat_heater_left": "0",
  "passenger_temp_setting": "22.2",
  "is_auto_conditioning_on": "0",
  "driver_temp_setting": "22.2",
  "outside_temp": "16.0",
  "seat_heater_rear_center": "0",
  "is_rear_defroster_on": "0",
  "seat_heater_rear_right_back": "",
  "smart_preconditioning": "",
  "seat_heater_right": "0",
  "fan_status": "0",
  "is_front_defroster_on": "0",
  "seat_heater_rear_left": "0",
  "gui_charge_rate_units": null,
  "gui_24_hour_time": null,
  "gui_temperature_units": null,
  "gui_range_display": null,
  "gui_distance_units": null,
  "sun_roof_installed": null,
  "rhd": "0",
  "remote_start_supported": "1",
  "homelink_nearby": "0",
  "parsed_calendar_supported": "1",
  "spoiler_type": "None",
  "ft": "0",
  "odometer": "4371.156405",
  "remote_start": "0",
  "pr": "0",
  "climate_keeper_mode": "off",
  "roof_color": "RoofColorGlass",
  "perf_config": "",
  "valet_mode": "0",
  "calendar_supported": "1",
  "pf": "0",
  "sun_roof_percent_open": "",
  "third_row_seats": "None",
  "seat_type": null,
  "api_version": "17",
  "rear_seat_heaters": "1",
  "rt": "0",
  "exterior_color": "MidnightSilver",
  "df": "0",
  "autopark_state": "ready",
  "sun_roof_state": "",
  "notifications_supported": "1",
  "vehicle_name": "Nebula++",
  "dr": "0",
  "autopark_style": null,
  "car_type": "model",
  "wheel_type": "Induction20Black",
  "locked": "1",
  "center_display_state": "0",
  "last_autopark_error": null,
  "car_version": "2021.12.25.7 30c3ea0",
  "defrost_mode": "0",
  "autopark_state_v2": null,
  "is_preconditioning": "0",
  "inside_tempF": "70",
  "driver_temp_settingF": "71",
  "outside_tempF": "60",
  "battery_heater": "0",
  "Notes": "",
  "odometerF": "None",
  "idleNumber": 4190,
  "sleepNumber": 0,
  "driveNumber": 0,
  "chargeNumber": 0,
  "polling": "",
  "idleTime": 42,
  "maxRange": "309.57",
  "left_temp_direction": "508",
  "max_avail_temp": "28.0",
  "is_climate_on": "0",
  "right_temp_direction": "510",
  "min_avail_temp": "15.0",
  "is_user_present": "0",
  "in_service": "0",
  "valet_pin_needed": null,
  "charge_port_led_color": "",
  "timestamp": null,
  "power": "0",
  "side_mirror_heaters": "",
  "wiper_blade_heater": "",
  "steering_wheel_heater": "",
  "elevation": "",
  "sentry_mode": "1",
  "fd_window": "0",
  "fp_window": "0",
  "rd_window": "0",
  "rp_window": "0",
  "measure": "imperial",
  "temperature": "F",
  "currency": "$",
  "carState": "Sentry",
  "location": "No Tagged Location Found",
  "rangeDisplay": "rated",
  "newVersion": "2021.12.25.7",
  "newVersionStatus": ""
}
```
