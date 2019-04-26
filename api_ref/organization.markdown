---
layout: api
title:  "API Reference: Societies and venues"
date:   2019-04-07 14:47:00 +0000
author: "George Bateman"
permalink: /api/organizations
---

Societies and venues follow a similar data model internally and the API returns structures that are also very similar. We refer to societies and venues together as "organizations" in some cases.

<div class="tabbed-content">
<span class="title active">Society JSON</span>
<div class="content" markdown="1">
<https://www.camdram.net/societies/cambridge-university-musical-theatre-society.json>
```javascript
{
    "id": 19,
    "name": "Cambridge University Musical Theatre Society",
    "description": "The Cambridge University Mus[...]",
    "image": { /* see show example */  },
    "facebook_id": "143157325784718",
    "short_name": "CUMTS",
    "slug": "cambridge-university-musical-theatre-society",
    "type": 0,          // ignore
    "_type": "society"  // ignore
}
```
</div>
<span class="title">Society XML</span>
<div class="content" markdown="1">
<https://www.camdram.net/societies/cambridge-university-musical-theatre-society.xml>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<society id="19" rel="society">
  <name>Cambridge University Musical Theatre Society</name>
  <description><![CDATA[The Cambridge University Musical Theatre...
  ...or just want to know more, please contact us.]]></description>
  <image rel="image">
    <id>4</id>
    <filename><![CDATA[85ef514ef8a551817068ee0408e15f05]]></filename>
    <created_at><![CDATA[2014-02-16T03:04:10+00:00]]></created_at>
    <width>1024</width>
    <height>546</height>
    <extension><![CDATA[png]]></extension>
    <type><![CDATA[image/png]]></type>
  </image>
  <facebook_id>143157325784718</facebook_id>
  <short_name>CUMTS</short_name>
  <slug>cambridge-university-musical-theatre-society</slug>
  <type>0</type> <!-- ignore, may be removed -->
  <link id="shows" rel="show" href="/societies/cambridge-university-musical-theatre-society/shows"/>
</society>
```
</div>
<span class="title">Venue JSON</span>
<div class="content" markdown="1">
<https://www.camdram.net/venues/adc-theatre.json>
```json
{
    "id": 29,
    "name": "ADC Theatre",
    "description": "See the ADC Theatre website at [L:www.adctheatre.com]",
    "image": {
        "id": 1775,
        "filename": "5c0d110e0c51d.png",
        "created_at": "2018-12-09T12:56:46+00:00",
        "width": 1024,
        "height": 1024,
        "extension": "png",
        "type": "image\/png",
        "_type": "image"
    },
    "facebook_id": "33348320992",
    "twitter_id": "36725639",
    "short_name": "ADC Theatre",
    "slug": "adc-theatre",
    "address": "Park Street,\nCambridge,\nCB5 8AS",
    "latitude": 52.208486948646,
    "longitude": 0.11986523866653,
    "type": 1,
    "_type": "venue"
}
```
</div>
<span class="title">Venue XML</span>
<div class="content" markdown="1">
<https://www.camdram.net/venues/adc-theatre.xml>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<venue id="29" rel="venue">
  <name>ADC Theatre</name>
  <description><![CDATA[See the ADC Theatre website at [L:www.adctheatre.com]]]></description>
  <image rel="image">
    <id>1775</id>
    <filename><![CDATA[5c0d110e0c51d.png]]></filename>
    <created_at><![CDATA[2018-12-09T12:56:46+00:00]]></created_at>
    <width>1024</width>
    <height>1024</height>
    <extension><![CDATA[png]]></extension>
    <type><![CDATA[image/png]]></type>
  </image>
  <facebook_id>33348320992</facebook_id>
  <twitter_id>36725639</twitter_id>
  <short_name>ADC Theatre</short_name>
  <slug>adc-theatre</slug>
  <address><![CDATA[Park Street,
Cambridge,
CB5 8AS]]></address>
  <latitude>52.208486948646</latitude>
  <longitude>0.11986523866653</longitude>
  <type>1</type> <!-- ignore -->
  <link id="shows" rel="show" href="/venues/adc-theatre/shows"/>
</venue>
```
</div></div>

### Basic keys
- `id` (int): constant for a given show.
- `name` (string): always present.
- `short_name` (string): an abbreviation for the name.
- `slug` (string): can be used to generate the society's or venue's URL. Always present.
- `description` (string): Specified in [Camdram Markdown](/api/markdown).

### URLs
- `facebook_id` (string): append this to `https://www.facebook.com/` to get the URL of their Facebook page
- `<link id="shows">` (unique to XML): the `href` attribute of this is `/{societies|venues}/{slug}/shows`, which is **not in fact a resolvable URL**. (Append `.json` or `.xml` to make it usable.) Users working with JSON can just construct the URL themselves as they already know the `slug`.

### Finding lists of associated shows

The URLs below allow relevant shows to be queried.
  * `https://www.camdram.net/{societies|venues}/{slug}/diary.{ics|json|xml}` return a [Diary](/api/diary) object. This takes query string parameters (i.e. `?key=value&key2=value` appended to the URL).
    * `from`. A date or datetime. Using `YYYY-MM-DD` or `YYYY-MM-DD HH:MM` format is recommended. Defaults to the current time.
    * `to`. A date or datetime, format as above. Defaults to one year after `from`.
  * `https://www.camdram.net/{societies|venues}/{slug}/shows.{json|xml}` return a list of [Show](/api/show) objects.
    * Takes `from` and `to` in the query string exactly as above.
  * `https://www.camdram.net/{societies|venues}/{slug}/events.{ics|json|xml}` (deprecated). This URL is provided for backward compatibility. It redirects to the diary URL above but doesn't allow you to pass a query string.

