I started by launching the instance, which remained available for 15 minutes.

Using Developer Tools (by pressing `F12` or right-clicking and selecting **Inspect**), I inspected the webpage to look for clues.

In the **Elements** tab, I noticed two script tags: `<script src="/static/js/xmlDetailsCheckPayload.js"></script>` and `<script src="/static/js/detailsCheck.js"></script>`, which indicated potential XML handling and suggested an XXE vulnerability.

![Screenshot (1173)](https://github.com/user-attachments/assets/7c120152-048d-4936-870d-2d82df0fe9e4)


Additionally, I found a form element: `<form class="detailForm" action="/data" method="POST"> (...) </form>`, revealing that the form data was sent as a `POST` request to the `/data` endpoint.

![Screenshot (1174)](https://github.com/user-attachments/assets/b8829568-9977-4074-a621-c23d28372a42)


Switching to the **Network** tab, I refreshed the page using `Ctrl + R` to monitor network traffic.
I observed that the JavaScript files `xmlDetailsCheckPayload.js` and `detailsCheck.js` were loaded from `http://saturn.picoctf.net:63374`, which provided the base URL.

![Screenshot (1179)](https://github.com/user-attachments/assets/8da6fff5-660d-4cbb-acfd-9dab433fa7d8)

![Screenshot (1180)](https://github.com/user-attachments/assets/71fb8846-5ad0-49a0-a710-867d8c0cd0a2)


By combining this base URL with the action `/data`, I constructed the target URL: `http://saturn.picoctf.net:63374/data`.

Using POSTMAN, I set up a `POST` request to the constructed URL, selected `raw` as the body type, and set the format to `XML` before pasting the payload.

![Screenshot (1178)](https://github.com/user-attachments/assets/38471c7a-a48e-44da-8cbe-e02cb5666ca0)


Upon sending the request, the server responded with the contents of `/etc/passwd`.

Scanning through the response, I successfully located the flag and completed the challenge.

**picoCTF{XML_3xtern@l_3nt1t1ty_e79a75d4}**
