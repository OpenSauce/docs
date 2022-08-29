You cannot use the new `initial_scan = "yes"/"no"/"only"` syntax with {% if page.name == "alter-changefeed.md" %}  `ALTER CHANGEFEED` {% else %}  
[`ALTER CHANGEFEED`](alter-changefeed.html) {% endif %} in v22.1. To ensure that you can modify a changefeed with the `initial_scan` options, use the previous syntax of `initial_scan`, `no_initial_scan`, and `initial_scan_only`.