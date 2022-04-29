```Docker on Windows machine:```

Setting up Telegraf, InfluxDB, LoudML and Grafana on Docker requires the creation of docker-compose.yml file that will pull the images and create a swarm of services that will share a common network.

docker-compose.yml file:
    - has services: telegraf, influxdb, loudml, grafana.
    - services pull images, name containers, define storage locations, order of how images are built into containers and which services can be accessed between each other.

```Explaining docker-compose.yml file:```
<details>
    <summary>version: "3.6"</summary>
    This refers to the version of docker-compose which has specific commands for each version.
</details>

<details>
    <summary>services:</summary>
    This contains all the 'services' representing each app in part.
</details>
    <details>
    <summary>loudml:</summary>
    This is the name of the service
    </details>
        <details>
        <summary>image: loudml/loudml:1.6.0</summary>
        This is the image from which the container is built.
        </details>
        <details>
        <summary>container_name: LoudML</summary>
        This is the name of the container created from the pulled image.
        </details>
        <details>
        <summary>init: true</summary>
        This makes sure the orphaned processes created following the exit of a parent node within the Unix processes             tree (each proces has a child except for the top-most process) are adopted by init so they do not become zombi           processes drawing resources and correctly responding with their status to the 'ps' command when queried so they         can be restarted if they become zombi.
        </details>
        <details>
        <summary>restart: always</summary>
        This restarts the container every time there is an error that stops the container from working properly.
        </details>
        <details>
    <summary>volumes:</summary> 
    This is the part where you define the storage for this specific container.
    </details>
    <details>
    <summary>- "D:/APPLICATIONS/LoudMLData/loudml/loudml.yml:/etc/loudml/config.yml:ro"</summary>
    This mounts a file ('bind mounts') from my computer on the container. It syncs changes from my loudml.yml file on my     laptop to the one in the container to persist configs.
    </details>
    <details>
    <summary>- var_loudml</summary>
    This just tells docker to manage the rest of the container storage by itself.
    </details>
    <details>
    <summary>ports:</summary>
    <details>
        <summary>- "8077:8077"</summary>
        This opens and binds the external-port(laptop):internal-port(docker).
    </details>
    <details>
        <summary>depends_on:</summary>
        - influxdb This tells docker to build influxdb container first.
    </details>
    </details>

      influxdb:
        image: influxdb
        container_name: InfluxDB
        restart: always
        ports:
          - "8086:8086"
        volumes:
          - var_influxdb

      telegraf:
        image: telegraf
        container_name: Telegraf
        restart: always
        volumes:
          - "D:/APPLICATIONS/LoudMLData/telegraf/telegraf.conf:/etc/telegraf/telegraf.conf:ro"
        depends_on:
          - influxdb
        links: 
          - "influxdb:trythis"----------------# This means that Telegraf can access InfluxDB service ('influxdb') using the service name 'influxdb' or 'trythis' as the hostname within the url: http://trythis:8086

      grafana:
        image: grafana/grafana-enterprise:8.2.0
        container_name: Grafana
        restart: always
        ports:
          - 3000:3000
        user: '472'---------------------------# This is the user ID for Grafana version >=7.3
        volumes:
          - var_grafana
        depends_on:
          - influxdb

    volumes:----------------------------------# Here you specify the volumes so they can be created on docker. The difference is that here you don't specify the path but just the name of the volume to be manged by docker.
      var_loudml:
      var_influxdb:
      var_grafana:
