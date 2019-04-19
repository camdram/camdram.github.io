---
layout: api
title:  "API Reference: Camdram-Flavoured Markdown"
date:   2019-04-07 14:47:00 +0000
author: "George Bateman"
permalink: /api/markdown
---

<strong>This is not our friendly guide to using Markdown!</strong> If you're only trying to format your show's blurb, [click here](/2019/04/07/using-markdown-on-camdram.html).

-----

Camdram allows users to enter show descriptions in plain text using Markdown. However, Markdown varies from implementation to implementation, and Camdram has to deal with legacy data, so this is not the whole story.

We use the [Parsedown](https://github.com/erusev/parsedown) library to convert Markdown to HTML, alongside a few other techniques for legacy reasons.

The relevant code (simplified from our [TextService.php](https://github.com/camdram/camdram/blob/master/src/Acts/CamdramBundle/Service/TextService.php)) is:

```php
$markdown_regexs = array(
    '/\[L:(www\.[a-zA-Z0-9\.:\\/\_\-\?\&]+)\]/'          => '[$1](http://$1)',
    '/\[L:([a-zA-Z0-9\.:\\/\_\-\?\&]+)\]/'               => '$1',
    '/\[L:(www\.[a-zA-Z0-9\.:\\/\_\-\?\&]+);([^\]]+)\]/' => '[$2](http://$1)',
    '/\[L:([a-zA-Z0-9\.:\\/\_\-\?\&]+);([^\]]+)\]/'      => '[$2]($1)',
    '/\[E:([a-zA-Z0-9\.@\_\-]+)\]/'                      => '[$1](mailto:$1)',
    '/\[E:([a-zA-Z0-9\.@\_\-]+);([^\]]+)\]/'             => '[$2](mailto:$1)',
    '/\[L:mailto\:([a-zA-Z0-9\.@\_\-]+)\]/'              => '[$1](mailto:$1)',
    '/\[L:mailto\:([a-zA-Z0-9\.@\_\-]+);([^\]]+)\]/'     => '[$2](mailto:$1)',
    # Making the most common HTML work
    '/<\/?b>/'                                           => '**',
    '/<\/?i>/'                                           => '*',
    '/<br ?\/?>/'                                        => "\n",
    '/<hr ?\/?>/'                                        => "\n_______\n",
    # Enforce sensble header levels in user-submitted content.
    # h1 → h3, hn → h(n-1) for 2 ≤ n < 6.
    '/(?m)^(#{2,5}[^#])/'                                => "#$1",
    '/(?m)^#([^#])/'                                     => "###$1",
);

$this->parsedown = new Parsedown();
$this->parsedown->setSafeMode(true);
$this->parsedown->setBreaksEnabled(true);
if ($strip_tags) {
    // FYI: using strip_tags like this is NOT a defence against XSS.
    // We use Parsedown's safe mode to block harmful code.
    $text = strip_tags($text, $this->allowed_tags);
}
$text = preg_replace(array_keys($this->markdown_regexs),
                     array_values($this->markdown_regexs), $text);
return $this->parsedown->text($text);
```

If you want to display properly formatted data from Camdram in your PHP application you can just use the above. In other languages, applying the regex list above followed by a GitHub-Flavoured markdown parser is likely to give perfectly acceptable results.

We are considering developing a script to convert Camdram Markdown to HTML using JavaScript in the future.
