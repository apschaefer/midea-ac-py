Forked from https://github.com/ruben0909/midea-ac-py which is forked from https://github.com/NeoAcheron/midea-ac-py and also using midea directory from https://github.com/Michael0yodi/midea-ac-py.

I experienced problems with Homeassistant docker builds .85 -> 0.93.2 trying to follow the original directions in NeoArcheron's wiki. It appears homeassistant changed the way it handles package requirements and none of the original files or forks worked for me.  I was finally able to get this to work after reading ruben0909's manifest.json and I modified that manifest.json format to include pycryptodome and midea as requirements. This causes an automatic fetch of the requirements to the deps/lib/python3.7 directory.  I still encountered errors after doing this. I needed to use Michael0yodi's version of midea 0.1.7 with Ruben0909's climate.py copy. 

Here are hopefully the details of how to get this working in docker.


Docker create instructions. Including this as I remapped using '-v' the /config directory (which in turn houses the deps and custom_components).

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

secrets.yaml:
midea_password: YOURPASSWORDHERE


To fix the library I overlay / copied the contents of the midea directory (from this fork, overlay it on the created deps directory from the manifest after initial startup of the container).  

e.g.:

cp $fork/midea/* /opt/docker/homeassistant/deps/lib/python3.7/site-packages/midea (or wherever your deps directory is). 
      
     
I don't know proper git etiquette (this is my first fork / edit with github), so preserving the comments below as this is not my original work. 

Original author(s) comments below: 
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
