# Q-SYS-Zoom-Room-REST-API

- Zoom Room REST API User Component
- Written by Glen Gorton
- This plugin was developed as best it could be, but the Zoom REST API has its limitations. Zoom were not receptive to improvements, instead directing me to the new Zoom Room Controller SDK. (https://developers.zoom.us/docs/rooms/controller/)
- No further development will occur on this Plugin.

## NOTES:
-- Zoom Server-to-Server OAuth App Configuration
-- This App needs to be created in order for Q-Sys Zoom Room scripts/plugins to authenticate with Zoom and download a token before the Zoom Room REST API command will function.
-- Below are the steps/configuration used when creating the app.
-- If the App needs to be recreated or the Account ID, Client ID, or Client Secret change, these credentials will need to be re-entered into the Zoom script/plugin on each Q-Sys Core controlling a Zoom Room.
-- https://developers.zoom.us/docs/internal-apps/create/
-- Take a note of your Secret Token during App Configuration.

-- Scopes added to App (14):
-- GET Requests (10):
-- Get a device profile = zoom_rooms:read:device_profile:admin
-- Get device information = zoom_rooms:read:device_profile:admin
-- Get Zoom Room profile = zoom_rooms:read:room:admin
-- Get Zoom Room sensor data = zoom_rooms:read:sensor_data:admin
-- Get Zoom Room settings = zoom_rooms:read:room_settings:admin
-- Get Zoom Rooms virtual controller URL = zoom_rooms:read:virtual_controller:admin
-- List device profiles = zoom_rooms:read:list_device_profiles:admin
-- List digital signage contents = zoom_rooms:read:list_digital_signage_contents:admin
-- List Zoom Room devices = zoom_rooms:read:list_devices:admin
-- List Zoom Rooms = zoom_rooms:read:list_rooms:admin
-- Note: ‘Get a device profile’ and ‘Get device information’ use the same Scope.

-- PATCH Requests (5):
-- Update a device profile = zoom_rooms:update:device_profile:admin
-- Update a Zoom Room profile = zoom_rooms:update:room:admin
-- Update E911 digital signage = zoom_rooms:update:room_controls:admin
-- Update Zoom Room settings = zoom_rooms:update:room_settings:admin
-- Use Zoom Room controls = zoom_rooms:update:room_control:admin
-- I have not set any of the Scopes as Optional. Assuming that by enabling them all it will allow for easier plugin development.
