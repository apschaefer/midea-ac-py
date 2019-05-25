Forked from https://github.com/ruben0909/midea-ac-py which is forked from https://github.com/NeoAcheron/midea-ac-py and also using midea directory from https://github.com/Michael0yodi/midea-ac-py.

Docker create instructions. Including this as I remapped using '-v' the /config and custom_components directories.

docker run \
    --init -d --name="home-assistant" \
    -v /opt/docker/homeassistant:/config \
    -v /etc/localtime:/etc/localtime:ro \
    --restart=unless-stopped \
    --name homeassistant \
    -e "TZ=America/New_York" \
    -p 8123:8123 \
homeassistant/home-assistant:latest


configuration.yaml:

    - platform: midea
      name: "MasterAC"
      app_key: 3742e9e5842d4ad59c2db887e12449f9
      username: 'username@email.com'
      password: !secret midea_password

Package dependency info. Using the manifest, deps are downloaded automatically, but from the wrong location. I used Y0da's midea package, not ruben0909's. It has a sleep fix among other items that appear to be required. To overlay I copied the contents of the midea directory (in this fork, overlaying the created deps directory from the manifest). 

cp midea/* /opt/docker/homeassistant/deps/lib/python3.7/site-packages/midea (or wherever your deps directory is). 
      
      



Original comments below: I don't know the pull etiquette (this is my first fork / edit with github). I see a problem with Homeassistant 0.93.2 (and likely other lower versions) and the manifest.json format. Modified accordingly. Note I had to also install dependencies manually:
------------------------------

## No more development
Unfortunately I do not have access to my Midea Air Conditioners as I have relocated to a new house. This means that I cannot reliably support this library due to not being able to test it with real hardware anymore.

# midea-ac-py 

This is a library to allow communicating to a Midea AC via the Midea Cloud.

This is a very early release, and comes without any guarantees. This is still an early work in progress and simply serves as a proof of concept.

This library would not have been possible if it wasn't for the amazing work done by @yitsushi and his Ruby based command line tool. 
You can find his work here: https://github.com/yitsushi/midea-air-condition
The reasons for me converting this to Python is that this library also serves as a platform component for Home Assistant.

## Wiki
Please visit the Wiki for device support and instruction on how to use this component: https://github.com/NeoAcheron/midea-ac-py/wiki 

## No more development
Unfortunately I do not have access to my Midea Air Conditioners as I have relocated to a new house. This means that I cannot reliably support this library due to not being able to test it with real hardware anymore.
