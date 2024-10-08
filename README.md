[![](https://data.jsdelivr.com/v1/package/npm/ad-auris-iframe-distribution/badge)](https://www.jsdelivr.com/package/npm/ad-auris-iframe-distribution)

## High level notes

#### Non-minified

`script-non-minified.js`
- non minified version of typical version 
- This is `production` ready
- To then minify see below for *Examples of use*
- transformation: `script-non-minfied.js` -> `script.js`

`preloaded-non-minified-script.js`
- non minified version of preload version
- the idea is the script goes above the iframe, which makes the logic a little harder 
but has possible speed games (not measured). 
- to then minify see below for *Examples of use*
- transformation: `preloaded-non-minified-script.js` -> `preloaded-script.js`

`canonical-url-non-minified-script.js`
- This is WIP for retriving the url via a specific property of a link node with a rel attribute as `canonical`
- This is a specific case for users that transfer sites and thus break url normalization ability
- transformation: `canonical-url-non-minified-script.js` -> `canonical-url.js`

`newspack-script-non-minified.js`
- newspack version
- difference is the nature of the iframe
- read more about [newspack](https://newspack.pub/) here


`newspack-preloaded-non-minified-script.js`
- newspack version
- difference is the nature of the iframe
- read more about [newspack](https://newspack.pub/) here
- This is a WIP, not currently being used

----------

#### Minified

`preloaded-script.js`
- script goes before iframe, attempt to load resource quickly

`script.js`
- script goes after, non blocking of other resources 
- This is `production` ready and is currently in use

`canonical-url.js`
- instead of using href, grabs canonical to compare with resource

----------

### Introduction 

This is the repository that contains scripts that powers our dynamic iframe 

Although there are multiple permutations of the scripts outlined above the general
flow of logic for each are all quite similar. 

#### General flow

1. retrieve the url of the page this script is on 
2. retrieve an identification of the organisation 
3. normalize url 
4. request dynamic widget service to find narration corresponding to url
    4a. if exists place into iframe source appropriate widget url
    4b. if does not exist, hide iframe


#### Why are there different permutations
The different permutations, exist due to different limitations or requirements
of different platforms (e.g. newspack vs regular webpage).

### Technologies
For each of the scripts we import axios at the top by copying it's minified version
to the top of the file.
This can be improved and is touched on in technical debt.

Outside of this, plain js is used

### Installation & Setup

The use case of this was meant to be a script 
```
<script>

<iframe 
    style="width: 100%;
    height: 100px;
    border: none;
    display: none" data-org=b716aa144485c0995f603a2510670284.28b6 allowfullscreen="false" allowtransparency allow="clipboard-read;
    clipboard-write" frameborder="0" id="ad-auris-iframe" scrolling="no">
</iframe>
<script 
    src="https://cdn.jsdelivr.net/npm/ad-auris-iframe-distribution@latest/script.js">
</script>
```

`https://cdn.jsdelivr.net/npm/ad-auris-iframe-distribution@latest/script.js`

We deliver this script via a cdn attached to `npm`
you can update this package using the [np](https://www.npmjs.com/package/np) package

### Scope of functionalities
This is meant to render the correct widget for the given article it finds it self in

### Examples of use
Currently being used in the dashboard , for our dynamic widget. Contact the team 
if you need help finding this on the dashboard.

There is a slight variation to that html snippet for [AMP](https://amp.dev/) sites due
to the restrictive nature of AMP rules. Learn more from our [docs](https://adauris.atlassian.net/wiki/spaces/AA/pages/147914762/Dynamic+widget+distribution+in+AMP.)

### Technical Debt
Axios is being used but instead can rely on more lower level network calls such as
[fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

This has only been tested manually, it would be good to create a "staging" area to test changes to 
these scripts

### TODOs

Because this script is being used inside of clients websites, the speed of this flow is important.
However there are no metrics on the speed outside of guessing that it is bad (by eye). 

All fault does not lie on this script, as it is simply the initiator to a bunch more other services
that all contribute delay.

### Where is testing needed?
You can trial this by looking at the different `temp*.html` files.
You will have to do some modification to the logic inside of whichever file you modify 

A testing environment that emulates how a user will use this script in conjunction with 
their website would be effective.
