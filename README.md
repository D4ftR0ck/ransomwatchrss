The script takes the data from https://ransomwatch.telemetry.ltd/#/ and integrates it into RSS feeds that you can feed into any RSS aggregator.

Require:
- The script must be put in a folder.
- Use crontab (if you want).
- Use jq (bash)

  ---
  
The script will check for new ransomware and put it in an RSS feed.

You just have to add this link: http://127.0.0.1:8555/monitoring.xml in an RSS feed aggregator
If you're using a local apache or nginx server, simply hide lines 83 and 84.

The date format you provided, "yyyy/MM/dd-HH:mm:ss.SSS. For example, 2013/02/27-13:57:21.123" (ISO8601)

The server is up for 600 seconds. You can modify it according to your needs.

I recommend activating a crontab every 24 hours.
