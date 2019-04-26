---
layout: api
title:  "API Reference: Show"
date:   2019-04-07 14:47:00 +0000
author: "George Bateman"
permalink: /api/show
---

Shows are the ultimate purpose of Camdram and so play a central role in most interaction with the API. They are expressed in terms of a description, one or more [Performance](/api/performance) objects, linked to zero or more societies, and also hold a variety of other data.

Let's take a reasonably complex show, the 2019 Lent Term Musical *Legally Blonde*, and break it down.


<div class="tabbed-content">
<span class="title active">JSON</span>
<div class="content" markdown="1">
<https://www.camdram.net/shows/2019-legally-blonde.json>
```javascript
{
    "other_society": "Cambridge University Amateur Dramatic Club",  // Deprecated
    "societies": [                       // This is an array, since it's common for multiple bodies to fund a show
        {
            "id": 1,
            "name": "Cambridge University Amateur Dramatic Club",
            "slug": "cambridge-university-amateur-dramatic-club"
        }
    ],
    "id": 6626,                          // A show's id will never change.
    "name": "Legally Blonde",
    "description": "The international [...].", // Long string, given in Camdram Markdown
    "image": {
        "id": 1854,
        "filename": "5c6cd9bf0ff4f.png",
        "created_at": "2019-02-20T04:38:23+00:00",
        "width": 1024,
        "height": 576,
        "extension": "png",
        "type": "image\/png",
        "_type": "image"
    },
    "slug": "2019-legally-blonde",        // The final part of the shows URL. Can be changed by the owner.
    "author": "Heather Hach",
    "other_venue": "ADC Theatre",         // Deprecated
    "category": "musical",
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
    "online_booking_url": "http:\/\/adctheatre.com\/omigod",
    "venue": {                   // Deprecated, since a show
        "id": 29,                // may tour multiple venues.
        "name": "ADC Theatre",
        "slug": "adc-theatre",
        "_type": "venue"
    },
    "society": {                 // Deprecated, since a show may
        "id": 1,                 // have multiple funding bodies
        "name": "Cambridge University Amateur Dramatic Club",
        "slug": "cambridge-university-amateur-dramatic-club",
        "_type": ""
    },
    "_links": {
        "self": "\/shows\/2019-legally-blonde",        // The URL of this show.
        "venue": "\/venues\/adc-theatre",              // Deprecated
        "society": "\/societies\/cambridge-university-amateur-dramatic-club" // Deprecated
    },
    "_type": "show"
}
```
</div><span class="title">XML</span>
<div class="content" markdown="1">
<https://www.camdram.net/shows/2019-legally-blonde.xml>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<show id="6626" rel="show">
  <other_society>Cambridge University Amateur Dramatic Club</other_society>
  <societies>
    <entry>
      <id>1</id>
      <name><![CDATA[Cambridge University Amateur Dramatic Club]]></name>
      <slug><![CDATA[cambridge-university-amateur-dramatic-club]]></slug>
    </entry>
  </societies>
  <name>Legally Blonde</name>
  <description><![CDATA[The international, award-winning musical Legally...

This laugh-out-loud musical will take you from the lavi...]]></description>
  <image rel="image">
    <id>1854</id>
    <filename><![CDATA[5c6cd9bf0ff4f.png]]></filename>
    <created_at><![CDATA[2019-02-20T04:38:23+00:00]]></created_at>
    <width>1024</width>
    <height>576</height>
    <extension><![CDATA[png]]></extension>
    <type><![CDATA[image/png]]></type>
  </image>
  <slug>2019-legally-blonde</slug>
  <author>Heather Hach</author>
  <other_venue>ADC Theatre</other_venue>
  <category>musical</category>
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
    <!-- further performances omitted... -->
  </performances>
  <online_booking_url>http://adctheatre.com/omigod</online_booking_url>
  <link id="venue" rel="venue" href="/venues/adc-theatre"/>
  <venue rel="venue">
    <id>29</id>
    <name>ADC Theatre</name>
    <slug>adc-theatre</slug>
  </venue>
  <link id="roles" rel="role" href="/shows/2019-legally-blonde/roles"/>
  <link id="society" rel="" href="/societies/cambridge-university-amateur-dramatic-club"/>
  <society rel="">
    <id>1</id>
    <name>Cambridge University Amateur Dramatic Club</name>
    <slug>cambridge-university-amateur-dramatic-club</slug>
  </society>
  <link id="self" rel="show" href="/shows/2019-legally-blonde"/>
</show>
```
</div>
</div>

Most of these keys are optional: if the administrator of the show doesn't specify a value, the key won't appear.

### Performances
See the [Performance object](/api/performance) reference.

### Societies and venues

This is an area where the API is likely to change in the near future. You'll notice that both the society and the venue were mentioned multiple times:

- `society` (object)` and other_society` (string): both of these are deprecated and will be removed. They specify just a single society, even when multiple funding bodies are listed. Old code using this should be updated as soon as possible.
- `societies`: an array of objects. This is the new way we specify societies. The major Cambridge societies are registered with us. They appear in the array as an object with `id`, `name` and `slug` (part of the URL) keys. Smaller, newer or external societies won't be registered with us, and appear as an object with `name` key only.
- `venue` (object), `other_venue` (string): also deprecated, since this makes no sense for tour shows.
- Instead, use the `venue` or `other_venue` key on the performance objects.

### URLs

A number of URLs appear in `_links` objects. These can be used to navigate the site; just prepend `https://www.camdram.net`. All these links will be current, regardless of how old the show is.

A booking URL, a Facebook id and a Twitter id can be associated with a show. Currently only the booking URL is available over the API.
- `online_booking_url` (string): used for ticket sales

Be aware that these may become dead links once a show is a few years or even months old; the ticket sales link shouldn't be displayed publicly once a show has ended!


