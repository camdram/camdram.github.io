---
layout: api
title:  "API Reference: Performance"
date:   2019-04-07 14:47:00 +0000
author: "George Bateman"
permalink: /api/performance
---

The Performance object refers to either a single performance of a show, or a repeating set of performances all occurring at the same time on consecutive days. You never need to request a single performance from the API: instead Performance objects are automatically included when you look up a [Show](/api/show) or the [Diary](/api/diary).

This example Performance was taken from 2019 Lent Term Musical *Legally Blonde*.

<div class="tabbed-content">
<span class="title active">JSON</span>
<div class="content" markdown="1">
<https://www.camdram.net/shows/2019-legally-blonde.json>
```javascript
{
    // ...
    "performances": [
        {
            "date_string": "19:45, Wed 13th March 2019 - Sat 16th March 2019",
            "start_date": "2019-03-13T00:00:00+00:00", // Not recommended
            "end_date": "2019-03-16T00:00:00+00:00",   // Not recommended
            "time": "1970-01-01T19:45:00+00:00",       // Not recommended
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
</div>
<span class="title">XML</span><div class="content" markdown="1">
<https://www.camdram.net/shows/2019-legally-blonde.xml>
```xml
<performances>
  <performance id="6849" rel="performance">
    <date_string>19:45, Wed 13th March 2019 - Sat 16th March 2019</date_string>
    <start_date>2019-03-13T00:00:00+00:00</start_date>
    <end_date>2019-03-16T00:00:00+00:00</end_date>
    <time>1970-01-01T19:45:00+00:00</time>
    <start_at>2019-03-13T19:45:00+00:00</start_at>
    <repeat_until>2019-03-16</repeat_until>
    <link id="show" rel="show" href="/shows/2019-legally-blonde"/>
    <show rel="show">
      <id>6626</id>
      <name>Legally Blonde</name>
      <slug>2019-legally-blonde</slug>
    </show>
    <link id="venue" rel="venue" href="/venues/adc-theatre"/>
    <venue rel="venue">
      <id>29</id>
      <name>ADC Theatre</name>
      <slug>adc-theatre</slug>
    </venue>
  </performance>
  <!-- ... -->
</performances>
```
</div>
</div>
## Dates

This refers to the first run from Wednesday to Saturday. Dates are provided as an *inclusive* range. A user-friendly string is available (`date_string`), alongside a `start_at` and `repeat_until` field. Some notes:
- Time offset from UTC (e.g. +01:00 for British Summer Time) is provided on `start_at` in ISO 8601 format. However this may not be the time offset for the whole run if the run crosses a GMT/BST boundary.
- `repeat_until` is provided as a date, not a date and time. If the performance is only one night then this is not provided.
- The exact format of the `date_string` key is subject to change without notice, so please avoid parsing it. All information is available in `start_at` and `repeat_until`.
- Not all data in Camdram is correct. Some performances may be very long or start in the distant past, which may cause your app to crash if you e.g. use memory proportional to the length of the run.

## Venue

This can be given as a string (for a venue not registered with Camdram) or an object. Either, neither or both may be present. If both are present, ignore `other_venue`.

- `venue`: object, relevant keys `id` (int), `name` (string) and `slug` (string)
- `other_venue`: string

## Show

A reference back to the show. Redundant if the performance was returned as part of a show description, but useful if provided as part of a [Diary](/api/diary).

- `show`: object, relevant keys `id` (int), `name` (string) and `slug` (string)

## Links

Internal (within Camdram) links to relevant objects. Relative URLs are given for the show (always) and venue (if a registered venue).

