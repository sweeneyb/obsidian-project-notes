## General upskilling

Cisco skills for all: https://skillsforall.com/
GCP upskilling: https://www.cloudskillsboost.google/

## External tools
https://www.shodan.io/

## Tools Doc
https://esphome.io/guides/getting_started_command_line.html

### e-paper doc
https://github.com/paperdink/PaperdInk-Library/tree/main/src/devices

## More Backend tools
https://directus.io/toolkit/editor

#### explored
https://firebase.google.com/
https://supabase.com/

## Distributed Events
### temporal
Golang framework for distributed events
https://docs.temporal.io/workflows#workflow-definition
https://docs.temporal.io/self-hosted-guide/upgrade-server

### Vector
Datadog observability pipelines
https://vector.dev/docs/setup/quickstart/
https://vector.dev/docs/reference/configuration/sources/kafka/

## BPMN
[[Up and running]]
https://camunda.com/download/self-managed/docker-compose/
https://github.com/camunda/camunda-platform

## ERP
ERPNext/Frappe - https://frappeframework.com/docs/user/en/bench/guides/diagnosing-the-scheduler
https://www.odoo.com/


## TSDB and monitoring
https://www.coursera.org/projects/server-application-monitoring#details
https://www.coursera.org/search?query=prometheus

## frigate & rstp

QR code for my setup.  Citation: https://cruxlab.com/blog/yi-camera-qr/.  Zoom in to 300%, else the camera doesn't have the resolution to grab the QR code.

```

/* Load the library */
<script src="https://cdn.rawgit.com/davidshimjs/qrcodejs/gh-pages/qrcode.min.js"></script>

/* Basic and simple one */
<div id="qrcode"></div>
<script type="text/javascript">
new QRCode(document.getElementById("qrcode"), {
    text: JSON.stringify({
        "ssid": "",
        "pwd": "",
        "res": "1080p",
        "rate": "3",
        "dur": "0",
        "url": "rtmp://192.168.3.106/yistream"
    })});
</script>

```

Run ```mediamtx``` on the arch box.  The defaults are fine to get the stream and publish on rstp.  Use vlc to open rstp://mao:8554/yistream

## Nginx proxy manager
https://nginxproxymanager.com/setup/#running-the-app

## Frigate
My docker-compose:

```
version: "3.9"

services:

  frigate:

    container_name: frigate

    privileged: true # this may not be necessary for all setups

    restart: unless-stopped

    image: ghcr.io/blakeblackshear/frigate:stable

    shm_size: "64mb" # update for your cameras based on calculation above

    devices:

      - /dev/dri/renderD128 # for intel hwaccel, needs to be updated for your hardware

    volumes:

      - "/etc/localtime:/etc/localtime:ro"

      - "/home/sweeneyb/projects/frigate/config/config.yml:/config/config.yml"

      - "/mnt/security/frigate:/media/frigate"

      - type: tmpfs # Optional: 1GB of memory, reduces SSD/SD Card wear

        target: /tmp/cache

        tmpfs:

          size: 1000000000

    ports:

      - "5000:5000"

 #     - "8554:8554" # RTSP feeds

      - "8555:8555/tcp" # WebRTC over tcp

      - "8555:8555/udp" # WebRTC over udp

    environment:

      FRIGATE_RTSP_PASSWORD: "xxx"
```
project is in ~/projects/frigate