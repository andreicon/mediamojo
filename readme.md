# MediaMojo

Media downloader using sonarr, radarr, jackett and transmission-daemon with openvpn support

## Usage

The instructions below apply to most vpn providers which allow downloading a `config.ovpn` file.

Before you go ahead and start the services, please check if your vpn provider allows port forwarding (it'd be nice to have). Set up a forward to `51413` in your account allowing all protocols.

Place your `config.ovpn` file at `./transmission/config/config.ovpn`.

If you would like to use filelist you need to uncomment the `jackett` service in the docker-compose file.

Starting the services is as easy as running `docker-compose up -d` in the project directory.

After the images have finished downloading and the services are up you can access them at:

http://localhost:8989 - Sonarr
http://localhost:7878 - Radarr
http://localhost:9091 - Transmission
http://localhost:9117 - Jackett (optional)

First you may want to check each service's config. Check the `./transmission/transmission-home/settings.json` file. Just before editing this you will have to take the transmission service offline with `docker-compose stop transmission`. Set a rpc user and rpc password and check everything else is set up according to your needs. Take it back up using `docker-compose start transmission`.

Go through sonarr and radarr configuration pages and make sure everything looks right:

- add the download client in the [Settings > Download Client](http://localhost:8989/settings/downloadclient) [pages](http://localhost:7878/settings/downloadclient). Click `+`, select transmission, type a name and click add. Make sure to do the same for both apps.
- add an indexer in [both](http://localhost:8989/settings/indexers) [services](http://localhost:7878/settings/indexers). You can use rarbg out of the box or use Torznab (Jackett) if you plan on using filelist. To add an indexer you will need to click the green `Add indexer` button from the menu just under the API key. Search for filelist (or your preferred tracker), click `configure` (the blue button with the wrench) add your credentials and save the indexer. Also take a second to check if the tracker has custom category ids, you may need to add these to sonarr and radarr later. Save this and click `Copy Torznab Feed` to get the link. Paste this along with your api key from jackett to both services and save the indexer.
- you may now begin adding content to your services

For more info on using these services you can check their wikis:

- https://github.com/sonarr/sonarr/wiki
- https://github.com/radarr/radarr/wiki
- https://github.com/jackett/jackett/wiki

If you're feeling adventurous (or just careless and lazy), you can use transmission without a vpn you can start the services with the `-f` flag use the `docker-compose.novpn.yml` services definition:

```bash
docker-compose up -d -f docker-compose.novpn.yml
```
