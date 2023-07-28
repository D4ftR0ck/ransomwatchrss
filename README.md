Prerequisite:
- The script must be put in a folder.
- Use crontab.
- Use jq (bash)

- Location folder (Each line of a file must contain a name, a GPS geolocation and a scan distance.)
- I recommend 500, 1000 or 1500 at the most. If you take more, change your ip.

The script will check for new ransomware and put it in an RSS feed.

You just have to add this link: http://127.0.0.1:8555/monitoring.xml in an RSS feed aggregator.

The server is up for 600 seconds. You can modify it according to your needs.

I advise you to activate the script every 24 hours.
