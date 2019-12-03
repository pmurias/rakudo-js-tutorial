The easiest way to try out rakudo.js is to try use [6pad](https://perl6.github.io/6pad)
If you load it in your browser you can run Raku code without installing anything.

To share or view example keep in mind 6pad can run gists.
If you create a gist like https://gist.github.com/pmurias/b5d2414766e7a87969cb26a9e67a72a2
6pad will load the code you put in main.p6 if you pass it like https://perl6.github.io/6pad/#b5d2414766e7a87969cb26a9e67a72a2

If you type this into 6pad and press run you will receive a generic welcome from rakudo.js

```perl6
say "Hello World'
```

Run it as https://perl6.github.io/6pad/#b5d2414766e7a87969cb26a9e67a72a2

Obviously that's fairly boring. Luckily ```EVAL(:lang<JavaScript>, ...)``` allows us to 
access all the underlying JavaScript implementation has to offer.

To pass values to Raku land the code needs to contain a return.

```perl6
my $document = EVAL(:lang<JavaScript>, 'return document');
```

6pad has a HTML tab so you can put 

```html
<span id='greeting'></span>
```

in it to have a place where we can display a more color full message.

You can call methods on the objects you get back from JavaScript
Raku strings are transparently converted to JavaScript ones

```perl6
my $greeting = $document.getElementById('greeting');

my $hello = $document.createElement('span');
my $world = $document.createElement('span');
```

JavaScript objects can be back to where they came from

```perl6
$hello.appendChild($document.createTextNode('Hello '));
$world.appendChild($document.createTextNode('World'));

$greeting.appendChild($hello);
$greeting.appendChild($world);
```

To access object properties of JavaScript objects use the $obj<propertyName> syntax.

```perl6
$hello<style><color> = 'red';
$world<style><color> = 'green';
```

See https://perl6.github.io/6pad/#ac109c38165187e5550bd6906f103f04 for a runnable DOM using hello world.

At some point you might want to deploy Raku script of merely showing it off in 6pad.
If you want to do so for browser use it's recommend you use the parcel bundler

```bash
mkdir hello-world
cd hello-world
yarn init
yarn add parcel@1.11.0 nqp-browser-runtime parcel-plugin-nqp parcel-plugin-async-perl6
```

or

```bash
mkdir hello-world
cd hello-world
npm init
npm add parcel@1.11.0 nqp-browser-runtime parcel-plugin-nqp parcel-plugin-async-perl6
```

Should setup your project.

Put this into main.p6

```perl6
my $document = EVAL(:lang<JavaScript>, 'return document');

my $greeting = $document.getElementById('greeting');

my $hello = $document.createElement('span');
my $world = $document.createElement('span');

$hello.appendChild($document.createTextNode('Hello '));
$world.appendChild($document.createTextNode('World'));

$greeting.appendChild($hello);
$greeting.appendChild($world);

$hello<style><color> = 'red';
$world<style><color> = 'green';
```

Put this into index.html

```html
<span id='greeting'></span>
<script src='main.p6></script>
```

Rakudo.js currently only target cutting edge browsers so add to your
package.json so that babel isn't used.

```
 "browserslist": [
    "last 2 Chrome versions"
  ]
```

Run

```bash
./node_module/bin/parcel index.html
```

to have parcel bundle up everything for browser use


