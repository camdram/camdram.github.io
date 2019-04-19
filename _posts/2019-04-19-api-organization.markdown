---
layout: api
title:  "API Reference: Societies and venues"
date:   2019-04-07 14:47:00 +0000
author: "George Bateman"
permalink: /api/organizations
---

Societies and venues follow a similar data model internally and the API returns structures that are also very similar. We refer to societies and venues together as "organizations" in some cases.

```javascript
https://www.camdram.net/societies/cambridge-university-musical-theatre-society.json
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

### Basic keys
- `id` (int): constant for a given show.
- `name` (string): always present.
- `short_name` (string): an abbreviation for the name.
- `slug` (string): can be used to generate the society's or venue's URL. Always present.
- `description` (string): Specified in GitHub-Flavoured Markdown

### URLs
- `facebook_id` (string): append this to `https://www.facebook.com/` to get the URL of their Facebook page

### Finding lists of associated shows

The URLs `https://www.camdram.net/{societies|venues}/{slug}/diary.{ics|json|xml}` return a [Diary](/api/diary) object.

