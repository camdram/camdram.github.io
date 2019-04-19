---
layout: api
title:  "API Reference: Performance"
date:   2019-04-07 14:47:00 +0000
author: "George Bateman"
permalink: /api/performance
---

The Performance object refers to either a single performance of a show, or a repeating set of performances all occurring at the same time on consecutive days. You never need to request a single performance from the API: instead Performance objects are automatically included when you look up a [Show](/api/show) or the [Diary](/api/diary).

This example Performance was taken from 2019 Lent Term Musical *Legally Blonde*.

```javascript
// https://www.camdram.net/shows/2019-legally-blonde.json
{
    // ...
    "performances": [
        {
            "date_string": "19:45, Wed 13th March 2019 - Sat 16th March 2019",
            "start_date": "2019-03-13T00:00:00+00:00", // Deprecated
            "end_date": "2019-03-16T00:00:00+00:00",   // Deprecated
            "time": "1970-01-01T19:45:00+00:00",       // Deprecated
            "id": 6849,
            "start_at": "2019-03-13T19:45:00+00:00",
            "repeat_until": "2019-03-16",
            "show": {
                "id": 6626,
                "name": "Legally Blonde",
                "slug": "2019-legally-blonde",
                "_type": "show"
            },
            "venue": {
                "id": 29,
                "name": "ADC Theatre",
                "slug": "adc-theatre",
                "_type": "venue"
            },
            "_links": {
                "show": "\/shows\/2019-legally-blonde",
                "venue": "\/venues\/adc-theatre"
            },
            "_type": "performance"
        },
        { ... }, // More of the same.
    ],
    // ...
}
```

