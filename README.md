# Q-SYS-Zoom-Room-REST-API

- Zoom Room REST API User Component
- Written by Glen Gorton
- This plugin was developed as best it could be, but the Zoom REST API has its limitations. Zoom were not receptive to improvements, instead directing me to the new Zoom Room Controller SDK. (https://developers.zoom.us/docs/rooms/controller/)
- No further development will occur on this Plugin.

## NOTES:
1. Zoom Server-to-Server OAuth App Configuration
   * This App needs to be created in order for Q-Sys Zoom Room scripts/plugins to authenticate with Zoom and download a token before the Zoom Room REST API command will function.
   * Below are the steps/configuration used when creating the app.
   * If the App needs to be recreated or the Account ID, Client ID, or Client Secret change, these credentials will need to be re-entered into the Zoom script/plugin on each Q-Sys Core controlling a Zoom Room.
   * https://developers.zoom.us/docs/internal-apps/create/
   * Take a note of your Secret Token during App Configuration.


-- Scopes added to App (14):
-- GET Requests (10):
- Get a device profile = zoom_rooms:read:device_profile:admin
- Get device information = zoom_rooms:read:device_profile:admin
- Get Zoom Room profile = zoom_rooms:read:room:admin
- Get Zoom Room sensor data = zoom_rooms:read:sensor_data:admin
- Get Zoom Room settings = zoom_rooms:read:room_settings:admin
- Get Zoom Rooms virtual controller URL = zoom_rooms:read:virtual_controller:admin
- List device profiles = zoom_rooms:read:list_device_profiles:admin
- List digital signage contents = zoom_rooms:read:list_digital_signage_contents:admin
- List Zoom Room devices = zoom_rooms:read:list_devices:admin
- List Zoom Rooms = zoom_rooms:read:list_rooms:admin
- Note: ‘Get a device profile’ and ‘Get device information’ use the same Scope.

-- PATCH Requests (5):
- Update a device profile = zoom_rooms:update:device_profile:admin
- Update a Zoom Room profile = zoom_rooms:update:room:admin
- Update E911 digital signage = zoom_rooms:update:room_controls:admin
- Update Zoom Room settings = zoom_rooms:update:room_settings:admin
- Use Zoom Room controls = zoom_rooms:update:room_control:admin

-- I have not set any of the Scopes as Optional. Assuming that by enabling them all it will allow for easier plugin development.


## REST API ISSUES:
1. Get Zoom Rooms virtual controller URL = https://api.zoom.us/v2/rooms/{roomId}/virtual_controller. Responds with below example. Found that "id" needs to be used rather than "room_id" as per the API. https://api.zoom.us/v2/rooms/{id}/virtual_controller
    HTML Data: 
    {
        "code": 300520002,
        "success": false,
        "message": "No Zoom Rooms found."
    }

2. zoomroom.thirdparty_meeting_join works for joining a MS Teams meeting, but zoomroom.meeting_leave and zoomroom.meeting_end won't Leave the meeting.
   * Unable to Leave a MS Teams meeting without physically pressing 'Leave' on the Controller.
   * Support ticket TS1910188 logged with Zoom. Zoom have advised I need to submit a feature request via Zoom Workplace -> Help Give Feedback. Submitted 22/08/2025.

3. zoomroom.share_content_start
   * When NOT in a meeting, appears to do nothing. Shouldn't this behave in the same was as the 'Share Content' button on the Controller which will share HDMI content (if 'Start HDMI content share manually on controller') is enabled in Room Settings?
   * When in a Zoom meeting, the Sharing pop up will come up and zoomroom.share_content_stop will close the popup.
   * Is there a way to Share Content -> HDMI via the REST API? (ie. Controller Interface = Share Content -> Use HDMI -> Share To Meeting)
   * Support ticket TS1910200 logged with Zoom.
   * Zoom have advised I need to submit a feature request via Zoom Workplace -> Help Give Feedback. Submitted 22/08/2025.

4. "Kiosk" zoom_room_type will not respond with "name" or "hide_room_in_contacts" (and many other properties when requesting the Room Profile (https://api.zoom.us/v2/rooms/{roomId}).
   * Feedback submitted to Zoom 29/08/2025.
   * If this gets resolved, delete the IF statement "if getdata.basic.name ~= nil then"

5. There does not appear to be a method to invite another Zoom Room to a meeting using the REST API. I expected I'd be able to invite a room using "room_id" or "name".
   * Feedback submitted to Zoom 05/09/2025.

6. There are API commands for accepting or declining an incoming meeting, but there's no way to know via the API whether a call is incoming or who's calling!
   * zoomroom.meeting_accept
   * zoomroom.meeting_decline
   * ZR-CSAPI has a notification (zEvent IncomingCallIndication) for incoming call and also details of the incoming caller.
   * Feedback submitted to Zoom 26/09/2025.

7. Meeting Password notification doesn't exist. When joining a meeting that requires a password, there is no notification/response/error from the API that advises a password is required.
   * The Zoom Controller will pop up requesting a password, and the ZR-CSAPI has a notification that meeting needs password: (MeetingNeedsPassword needsPassword: true)
   * Using the REST API, there is no way to inform the end user (or the control system) that a password is required to join the meeting.
   * The room "status" remains as "Available" so there's no way to know what state the meeting is in.
   * zoomroom.meeting_leave needs to be sent to exit out of the passcode entry screen on the Controller.
   * Feedback submitted to Zoom 26/09/2025.

8. Mic & Camera Mute states.
   * There are PATCH requests to mute/unmute the microphone and camera. Video and audio mute on and off:
   * zoomroom.mute
   * zoomroom.unmute
   * zoomroom.video_mute
   * zoomroom.video_unmute
   * But there is no GET request to discover the mute state of the microphone or camera.
   * The ZR-CSAPI utilises "zConfiguration Call Microphone Mute - In-Meeting" and "zConfiguration Call Camera Mute - In-Meeting" to GET and SET mute states.
   * Feedback submitted to Zoom 26/09/2025.
