# Octopus Germany Integration for Home Assistant

This custom component integrates Octopus Germany services with Home Assistant, providing access to your energy account data, electricity prices, device control, and vehicle charging preferences.


[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/integration)
![installation_badge](https://img.shields.io/badge/dynamic/json?color=41BDF5&logo=home-assistant&label=integration%20usage&suffix=%20installs&cacheSeconds=15600&url=https://analytics.home-assistant.io/custom_integrations.json&query=$.octopus_germany.total)

This integration is in no way affiliated with Octopus Energy.

If you find this useful and are planning on moving to Octopus Energy Germany, why not use my [referral link](https://share.octopusenergy.de/free-cat-744)?

## Features

- Account information display with electricity balance
- Support for multiple accounts under one integration
- Current electricity tariff prices
- Support for all ledger types:
  - Electricity ledgers
  - Gas ledgers
  - Heat ledgers
  - Other ledger types
- Support for Octopus tariff types:
  - Simple tariffs (fixed rate)
  - Time of Use tariffs (different rates at different times)
  - Heat tariffs (for heat pumps)
- Device smart control (suspend/unsuspend charging)
- Electric vehicle charging preferences management

## Installation

### HACS (Home Assistant Community Store)

[![hacs_badge](https://img.shields.io/badge/HACS-Default-41BDF5.svg?style=for-the-badge)](https://github.com/hacs/integration)

1. Add this repository as a custom repository in HACS
2. Search for ["Octopus Germany"](https://my.home-assistant.io/redirect/hacs_repository/?owner=thecem&repository=octopus_germany&category=integration) in the HACS integrations
3. Install the integration
4. Restart Home Assistant
5. Add the integration via the UI under **Settings** > **Devices & Services** > **Add Integration**

### Manual Installation

1. Copy the `octopus_germany` directory to your Home Assistant `custom_components` directory
2. Restart Home Assistant
3. Add the integration via the UI under **Settings** > **Devices & Services** > **Add Integration**

## Configuration

The integration is configured via the Home Assistant UI:

1. Navigate to **Settings** > **Devices & Services**
2. Click **+ ADD INTEGRATION** and search for "Octopus Germany"
3. Enter your Octopus Energy Germany email and password
4. The integration will automatically fetch your account number and set up the entities

## Entities

### Sensors

#### Electricity Price Sensor

- **Entity ID**: `sensor.octopus_<account_number>_electricity_price`
- **Description**: Shows the current electricity price in €/kWh
- **Attributes**:
  - `code`: Product code
  - `name`: Product name
  - `description`: Product description
  - `type`: Product type (Simple or TimeOfUse)
  - `valid_from`: Start date of validity
  - `valid_to`: End date of validity

### Switches

#### Device Smart Control

- **Entity ID**: `switch.octopus_<account_number>_device_smart_control`
- **Description**: Controls whether smart charging is enabled for your vehicle
- **Actions**:
  - Turn **ON** to enable smart charging (unsuspend)
  - Turn **OFF** to disable smart charging (suspend)
- **Attributes**:
  - `device_id`: Internal ID of the connected device
  - `name`: Name of the device
  - `model`: Vehicle model (if available)
  - `battery_size`: Battery capacity (if available)
  - `provider`: Device provider
  - `status`: Current status of the device
  - `last_updated`: Timestamp of the last update

## Services

### Set Vehicle Charge Preferences

- **Service ID**: `octopus_germany.set_vehicle_charge_preferences`
- **Description**: Configure your vehicle's charging preferences
- **Parameters**:
  - `account_number` (optional): Your Octopus Energy account number (uses account from configuration if not specified)
  - `weekday_target_soc` (required): Target state of charge (in %) for weekdays
  - `weekend_target_soc` (required): Target state of charge (in %) for weekends
  - `weekday_target_time` (required): Target time for weekday charging (HH:MM)
  - `weekend_target_time` (required): Target time for weekend charging (HH:MM)

**Example:**

```yaml
# Example automation to set vehicle charging preferences to 80% by 7:30 AM on weekdays and 90% by 9:00 AM on weekends
service: octopus_germany.set_vehicle_charge_preferences
data:
  weekday_target_soc: 80
  weekend_target_soc: 90
  weekday_target_time: "07:30"
  weekend_target_time: "09:00"
```

## Debugging

If you encounter issues, you can enable debug logging by adding the following to your `configuration.yaml`:

```yaml
logger:
  logs:
    custom_components.octopus_germany: debug
```

## Support

For bug reports and feature requests, please open an issue on the GitHub repository.
Before raising anything, please read through the [discussion](https://thecem.github.io/octopus_germany/discussions).
If you have found a bug or have a feature request please [raise it](https://thecem.github.io/octopus_germany/issues) using the appropriate report template.

## Sponsorship

If you are enjoying the integration, why not use my [referral link](https://share.octopusenergy.de/free-cat-744)

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Disclaimer

This integration is not officially affiliated with Octopus Energy Germany. Use at your own risk.
