---
title: "My Scripts"
image:
  path: /assets/img/logo_script.png
...

## Remove MP3 Metadata

Install the `music-tag` package:

```bash
pip install music-tag
```

The following script removes all metadata from an MP3 file:

```python
from mutagen.mp3 import MP3

mp3 = MP3("FILE_NAME.mp3")
mp3.delete()
mp3.save()
```

A more complete implementation is available
[here]({{site.baseurl}}/assets/lib/mp3_tag_cleaner.py).

## Battery Alert

To reduce battery wear, the charge level can be maintained approximately
between 20% and 85%.

A Bash script checks the battery level at regular intervals and sends a notification
when it falls below or rises above configurable thresholds.

> The battery monitoring script is available
> [here]({{site.baseurl}}/assets/lib/battery_health).
{: .prompt-info }

The script behavior can be configured through the following variables:

- `MAX_BATTERY`: maximum charge threshold
- `MIN_BATTERY`: minimum charge threshold
- `SLEEP_TIME`: interval between checks

Make the script executable:

```bash
chmod u+x battery_health
```

To run it after login, add the absolute script path to the desktop environment's
startup applications.

> Prefer `/home/USERNAME` or `"$HOME"` instead of `~` when configuring startup
> applications or services.
{: .prompt-info }
