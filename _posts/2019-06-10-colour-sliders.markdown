---
layout: post
title:  "How to over-engineer a colour slider"
date:   2019-05-10 12:00:00 +0100
author: "George Bateman"
---

<style>
h1::after { content: ':';}
blockquote { color: #222; font-style: normal; }
.mathpre {
margin-bottom: 15px;
white-space: pre;
font-family: monospace;
line-height: 1;}
</style>

## And how to procrastinate from revision with coordinate transformations.

Earlier in term we introduced colour sliders for shows and organizations. These let you set a theme for your page and give it an identity that's distinct from Camdram orange. This came with a caveat though:

> If you pick a pale colour it will be darkened in some places for legibility.

How should we do that? Legibility is a difficult thing to assess objectively. Perfectly clear to me on my laptop might mean you have to squint at your phone. There are multiple types of colourblindness to consider. And without my glasses on, no colour scheme at all is legible. If only someone had done detailed experiments and some up with a definitive answer to what's legible...

## Success Criterion 1.4.3: Contrast (Minimum)

The Web Content Accessibility Guidelines set down how to make a website accessible, across a range of guidelines. One of the areas covered is colour schemes, and how to avoid them interfering with users' vision. There's more to this than I can type in a blog post: [WCAG's Understanding 1.4.3](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum) cites more papers if that's your thing. The upshot of it all is, to cater to a reasonable audience ("AA"), there must be a 4.5:1 contrast ratio between normal-sized text and its background. Furthermore, if you're using colour to distinguish links from other text, there should be a 3:1 ratio between two. That's all very simple, you'd think: so what's the contrast ratio of <span style="background: #fae; color: #639;">pale pink to dark purple</span>? It turns out to depend on "relative luminance": for relative luminance values *Y*<sub>1</sub> and *Y*<sub>2</sub>, the contrast ratio is defined as
<div class="mathpre">Y₁ + 0.05
—————————
Y₂ + 0.05</div>

## Colour spaces

A point in a space (whether physical or abstract, like a colour) can be described in a coordinate system. Any position can be given as a latitude, longitude and altitude; or in terms of its position from me in any three numbers you like to choose. Different coordinate systems are useful in different situations, and we can convert values between coordinate systems.

Colour is usually described in a 3D space. Our art teachers told us we could make any colour from some ratio of red, yellow and blue paint. Computer graphics generally work in red, green and blue. Fundamentally, this is all getting transformed into the response of (usually) three types of cone cell in our eyes—L, M, S, meaning long, medium, and short wavelength—each responding to a different range of wavelengths of light. A number of colour spaces exist with different uses: all just different coordinate systems for colours.

(There are probably errors in the discussion below; it was cobbled together from a combination of Wikipedia and the standards documents. The code works, but colour is hard!)

### sRGB

The basic colour space. You might not have heard of it but you've almost certainly used it: this is the system typically used when picking colours with red, green and blue options. There's a limit to the range of the numbers: either 0–1 (for nice algebra) or 0–255 (for the real world; one byte). This makes it a fairly limited space. It doesn't cover anywhere near the range of colours your eyes do, but it *is* easy for basically any monitor to display. It corresponds pretty closely to how most screens work: displaying little red, green and blue subpixels with different intensities.

### HSL

More intuitive than sRGB, but with a close mathematical link to it. HSL takes the sRGB space and turns it into "hue" (an angle from 0–360° on the colour wheel) "saturation" (0 for grey, 1 for intense) and "luminosity" (0 for black, 1 for white). There's no link here to how our eyes work: it just follows [a simple set of formulae](https://en.wikipedia.org/w/index.php?title=HSL_and_HSV&oldid=890683083#RGB_to_HSL_and_HSV):

* *H* is a piecewise function of all of *r*, *g*, *b*.
* *S* = [max(*r*, *g*, *b*) - min(*r*, *g*, *b*)] ÷ max(*r*, *g*, *b*)
* *L* = [max(*r*, *g*, *b*) + min(*r*, *g*, *b*)] ÷ 2

This is really intuitive for people to use. You just slide your three sliders to select roughly what colour you want, and how bright you want it. No fuss. These are the sliders on show edit pages. (I decided to hide the numbers on our show edit page to keep things very simple.)

Neither of these two spaces tell us perceived brightness, really. The *L* in HSL is meaningless: *L* = ½ blue is much darker than *L* = ½ green, simply because our eyes are much less sensitive to it.

### XYZ (CIE 1931)

The least imaginatively named but most interesting of the coordinates. The XYZ space, based on real experiments and published in 1931, describes colour rather confusingly[[source]](https://reference.wolfram.com/language/ref/XYZColor.html) as

* *X*: green and red.
* *Y*: approximate luminance.
* *Z*: blue. Roughly.

This is great for biological accuracy. The *Y* value is what we use for relative luminance[[source]](https://www.w3.org/TR/2008/REC-WCAG20-20081211/#relativeluminancedef). Mathematically though, it's rather tedious to go between sRGB and XYZ; we have to first convert from sRGB (*r*, *g*, *b*) to linear RGB (*R*, *G*, *B*) using an awkward formula:

<div class="mathpre">
    ⎧   r / 12.92          r ≤ 0.03928
R = ⎨  ⎡<u>r + 0.055</u>⎤<small>2.4</small>
    ⎩  ⎣  1.055  ⎦         otherwise

(0 ≤ r ≤ 1, 0 ≤ R ≤ 1; same formula for green, blue)
</div>

then multiply the linear RGB coordinates by [a 3×3 matrix](https://en.wikipedia.org/w/index.php?title=SRGB&oldid=897666168#The_forward_transformation_(CIE_XYZ_to_sRGB)) to finally end up in XYZ space. Going backwards, there's a second problem: many points in XYZ space just don't exist in sRGB; they come out with *r*, *g*, or *b* out of their 0–1 range.

## Adjusting luminance..inosity..?

If you've followed me this far then the solution should seem simple enough. User picks a colour by HSL, this gets converted into sRGB, then into linear RGB, and finally into XYZ. The *Y* coordinate gets adjusted to whatever the rules say it should be, then we reverse all the transformations and end up with our legible text colour. This sounds like it'll work, but there are two major problems. Not only does that leave the risk of moving outside the sRGB space, it also turns out not to preserve the same colour at all.

<div style="width: 100%; height: 3em; color: #fff; background: linear-gradient(to right, #000, #000 30%, #f559bb 30%, #e192b7, #ccb7b4, #b3d4b0, #94edac, #69ffa8 80%, #000 80%, #000)"><i>Y</i> = 0<span style="float: right"><i>Y</i> = 1</span></div>

The bar above shows a slice through XYZ space at X = Z = ½. Clearly, it's not all the same colour, and in the black regions the colour has fallen outside of sRGB space altogether. If adjusting luminance (*Y*) is a lost cause, why not try adjusting luminosity (*L*, from HSL), until the luminance ends up being right? That's the solution I ultimately chose, just reducing *L* in steps until *Y* was low enough. Where it's necessary to lighten the colour, it's mixed with white until *Y* is high enough.

## Putting it all together

So, the situation finally is:
* We need to satisfy the equation for contrast ratio against both black text (*Y* = 0, 3:1 ratio) and white background (*Y* = 1, 4.5:1 ratio. This sets a constraint of 0.1 ≤ *Y* ≤ 0.1833.
* We can darken text by shifting it down the *L* axis of HSL space. It turns out you can do this by multiplying *r*, *g*, and *b* all by the same constant e.g. 0.95.
* We can lighten text a similar way, but that wouldn't work for very dark colours: *r* = 0.01, *g* = *b* = 0 (black with a tiny bit of red) would become <span style="color:#b60000">*r* = 0.72, *g* = *b* = 0, a fairly strong red</span>. The blue equivalent comes out at *b* = 1.15, outside the usable space. Instead, a constant is added to all components (mixing with white).
* Since the algebra is complex the process is iterative: repeatedly using a small constant until we're in the 0.1 ≤ *Y* ≤ 0.1833 range.

## The code

Having finally worked out how to lighten a colour in the most complex way conceivable, all that remained was to code it up in PHP. There's a bit more stuff going on in here, to get the colours in and out of hex notation. Some of the constants have been multiplied up to work with sRGB in the 0–255 range rather than the 0–1 range used above. Only the Y of the XYZ model is ever calculated.

```php
// This code is © George Bateman, 2019, and is licensed
// under the GNU General Public License v2.0.
/**
 * This function takes a six-digit hex color and returns an array
 * of hex colors which are accessible. E.g.
 * offer a contrast ratio of 3:1 and 4.5:1 against white, as defined at
 * WCAG 2.0 § 1.4.3 (https://www.w3.org/TR/WCAG20/#visual-audio-contrast-contrast).
 */
public function genWcagColors(string $hexColor): array
{
    $num = hexdec($hexColor);
    $result = ['raw' => '#' . substr('000000' . dechex($num), -6)];
    $rgb = [$num >> 16, ($num >> 8) & 255, $num & 255];
    $strictMinLum = 0.1;    # 3:1 against black.
    $strictMaxLum = 0.1833; # Level AA, < 18pt text vs. white.

    // In the interest of a simple algorithm I am deliberately mixing
    // two color models here. Multiplying $rgb by a constant lightens
    // it in the HSL model, while the luminance value it's being compared
    // to is based on a far more sophisticated approach.
    $rawMaxLum    = 0.5;       # Arbitrary luminance cap.
    // $laxMaxLum = 0.3;       # Level AA, ≥ 18pt text vs. white.
    while ($rawMaxLum < self::rgbToLum($rgb)) {
        $rgb[0] *= 0.95; $rgb[1] *= 0.95; $rgb[2] *= 0.95;
    }
    $result['raw'] = '#' . substr('000000' . dechex((int)$rgb[0] << 16 | (int)$rgb[1] << 8 | (int)$rgb[2]), -6);
    while ($strictMaxLum < self::rgbToLum($rgb)) {
        $rgb[0] *= 0.95; $rgb[1] *= 0.95; $rgb[2] *= 0.95;
    }
    while ($strictMinLum > self::rgbToLum($rgb)) {
        // This is mixing with white not lightening, so it won't infinite loop for black.
        $rgb[0] += 5; $rgb[1] += 5; $rgb[2] += 5;
    }
    $result['smalltext'] = '#' . substr('000000' . dechex((int)$rgb[0] << 16 | (int)$rgb[1] << 8 | (int)$rgb[2]), -6);
    return $result;
}
private static function rgbToLum(array $rgb): float {
    $RGB = [];
    for ($i = 0; $i < 3; $i++) {
        $RGB[] = $rgb[$i] <= 10 ? $rgb[$i] / 3294.6 : (($rgb[$i] + 14.025) / 269.025) ** 2.4;
    }
    return 0.2126*$RGB[0] + 0.7152*$RGB[1] + 0.0722*$RGB[2];
}
```
