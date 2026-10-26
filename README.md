OHDrive – openHAB for Android Auto

Bring your openHAB smart home to your car.

OHDrive is an Android Auto application that provides convenient access to selected openHAB devices, controls, and sensor information directly from your vehicle's infotainment display.

Whether you want to open your garage door, operate your entrance gate, check the outdoor temperature, or monitor your smart home, OHDrive connects your vehicle with your existing openHAB installation.

Current development version: 2.7

🚗 What is OHDrive?

OHDrive is designed for openHAB users who want to interact with their smart home while using Android Auto.

Instead of navigating through a full smart home dashboard, OHDrive presents selected openHAB items through a simplified, vehicle-oriented interface.

The application uses your existing openHAB installation and its REST API. No additional home automation platform or cloud service is required.

✨ Key Features

🏠 Smart Home Control

Control selected openHAB devices directly from your vehicle:

Open and close garage doors

Operate entrance gates

Trigger electric door openers

Control smart switches and relays

Trigger predefined openHAB rules and actions

Integrate Shelly devices and other equipment supported by openHAB

Controls are based on standard openHAB items, making OHDrive independent of specific smart home hardware manufacturers.

🌡️ Live Smart Home Information

Display information from your openHAB installation, including:

Indoor and outdoor temperatures

Weather conditions

Rain detection status

Garage and gate status

Device and automation status

Robotic lawn mower information

Custom sensor values

The available information depends on the items configured in your openHAB installation.

⚡ Real-Time Status Updates

OHDrive supports live status monitoring using openHAB's event stream.

Features include:

Server-Sent Events (SSE)

Monitoring of item state changes

Status synchronization after switching actions

Periodic refresh as a fallback

Detection of unexpectedly long relay activation

The current implementation uses a 30-second periodic refresh alongside event-driven updates.

🔘 Momentary Switch Support

OHDrive supports impulse-based controls, especially useful for garage doors, entrance gates, and electric door openers.

A typical openHAB configuration uses a switch item with an impulse command.

This allows OHDrive to trigger an action without requiring the user to manually reset the switch.

The current implementation also includes monitoring for relay activation exceeding 15 seconds.

🧩 Flexible openHAB Integration

OHDrive is designed around openHAB's standard interfaces:

openHAB REST API

openHAB Sitemap

Standard openHAB items

Switch commands

Item state monitoring

SSE event subscriptions

This approach makes the application suitable for different openHAB installations without requiring dedicated integrations for individual hardware brands.

🔐 Remote Access via VPN

OHDrive can communicate with an openHAB server over a private VPN connection.

A typical setup consists of:

An openHAB server running at home

An Android smartphone connected to the vehicle

Android Auto running on the infotainment display

An active VPN connection to the home network

OHDrive communicating with the openHAB REST API

A direct public internet connection to the openHAB server is not necessary.

For security, VPN-based access is recommended instead of exposing the openHAB REST API directly to the internet.

📱 Android Auto Interface

OHDrive focuses on a simple interface suitable for use in a vehicle.

The application is intended to provide quick access to selected functions without displaying the complexity of a complete openHAB administration interface.

Example functions:

Function

Description

Garage

Trigger the garage door

Entrance Gate

Operate the property entrance gate

Front Door

Trigger the electric door opener

Temperature

Display current temperature

Weather

Display selected weather information

Lawn Mower

Monitor robotic mower status

The actual controls depend on the openHAB configuration.

⚙️ openHAB Configuration

OHDrive uses a dedicated openHAB Sitemap to define the items available in the vehicle.

Example Sitemap:

sitemap ohdrive label="OHDrive" {
    Frame label="Home Controls" {
        Switch item=Garage_Impulse
            mappings=[ON="Impulse"]

        Switch item=Entrance_Gate
            mappings=[ON="Impulse"]

        Switch item=Front_Door
            mappings=[ON="Impulse"]
    }

    Frame label="Home Information" {
        Text item=Outdoor_Temperature
        Text item=Indoor_Temperature
        Text item=Rain_Status
    }
}

The example item names must be replaced with the corresponding items from your own openHAB installation.

The Sitemap is intended to provide a dedicated, simplified selection of functions for the vehicle.

🔌 Supported Devices

OHDrive is not restricted to a particular device manufacturer.

Any device that can be represented and controlled through supported openHAB items may potentially be integrated.

Examples include:

Shelly smart relays

Garage door controllers

Electric gates

Smart switches

Temperature sensors

Weather stations

Door and gate contacts

Robotic lawn mowers

Other openHAB-compatible devices

Device compatibility depends on the openHAB binding, item configuration, and the functionality implemented in OHDrive.

🛠️ Technical Overview

Component

Technology

Mobile platform

Android

Vehicle integration

Android Auto

Smart home platform

openHAB

Communication

HTTP/HTTPS REST API

Live updates

Server-Sent Events

Configuration

openHAB Sitemap and Items

Remote connectivity

VPN

Current development version

2.7

OHDrive is developed as an Android application using Android Studio.

The application communicates with the existing openHAB installation rather than requiring a separate automation server.

🔒 Security Considerations

OHDrive can control physical devices such as garage doors and entrance gates.

For this reason, secure configuration is particularly important.

Recommended precautions:

Use a trusted VPN for remote access

Protect the openHAB REST API with appropriate authentication

Avoid exposing the REST API publicly

Limit access to necessary items and functions where possible

Use momentary commands for door and gate operations

Configure appropriate safety mechanisms within openHAB

Do not rely on the Android Auto application as the sole safety mechanism

Physical device safety must always be handled by the relevant hardware and automation configuration.

🚀 Installation

OHDrive is currently under development.

The intended installation process is:

Install and configure openHAB.

Create the required openHAB items.

Configure the dedicated ohdrive Sitemap.

Establish network access to your openHAB server.

Install the OHDrive Android application.

Configure the connection to your openHAB installation.

Connect your smartphone to a compatible Android Auto vehicle.

Launch OHDrive from the Android Auto interface, where supported.

Installation packages and detailed setup instructions will be provided with the corresponding releases.

⚠️ Disclaimer

OHDrive is an independent project for the openHAB community.

It is not an official openHAB application and is not affiliated with or endorsed by the openHAB Foundation, Google, or vehicle manufacturers.

Android Auto and Android are trademarks of Google LLC.

Users are responsible for the secure configuration of their openHAB systems and connected physical devices.

Always follow local regulations and vehicle safety requirements. Do not interact with smart home controls while driving when doing so could cause distraction or unsafe operation.

🤝 Contributions and Feedback

OHDrive is intended to evolve into a useful solution for the wider openHAB community.

Feedback, testing, feature suggestions, and contributions are welcome.

The long-term goal is to make selected openHAB functions conveniently accessible from compatible vehicle infotainment systems.

OHDrive – Your smart home, connected to your car.

Developed for the openHAB community.
