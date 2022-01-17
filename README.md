# Conbee ii on Synology NAS (DSM 7) using Docker and Docker-Compose

1. Connect to SSH.

2. Create a folder where all configurations will later be located.
``` mkdir -p /deconz/opt```

3. Get root rights.
```sudo su```

4. Paste this to Terminal:
```_modprobe usbserial_ && modprobe ftdi_sio_ && modprobe cdc-acm_```

5. Exit now we don't anymore root rights.
```exit```

6. Create now in **deconz** folder a file with the name **docker-compose.yml** paste this and set your settings.

```version: "2.1"
services:
  deconz:
    image: marthoc/deconz:stable
    container_name: deconz
    network_mode: host
    environment:
      - PUID=1026 # Here comes yours
      - PGID=100 # Here comes yours
      - TZ=Europe/Berlin
      - DECONZ_WEB_PORT=8124
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /volume2/docker/deconz/opt:/root/.local/share/dresden-elektronik/deCONZ # Change to your volume.
    devices:
      - /dev/ttyACM0:/dev/ttyACM0
    restart: always
```

7. Now run docker-compose (you must be in the folder **deconz** where the file **docker-compose.yml** is located.)
```docker-compose up -d```

That's it now you can call the gateway server at 192.168.XXX.XXX:8124 and/or add it to Home Assistant or similar.

I hope this helped you.
