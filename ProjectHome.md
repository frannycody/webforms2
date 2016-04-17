**Update, 7 November 2010:** Moving to GitHub: https://github.com/westonruter/webforms2

**Update, 3 June 2009:** _Now that the Web Forms 2.0 has been edited into HTML5, this project will seek to implement as much of the HTML5 [Forms section](http://www.whatwg.org/specs/web-apps/current-work/multipage/forms.html) as possible. This will be a complete rewrite of the codebase. Restarting this project will commence as time permits._

A cross-browser implementation of the WHATWG Web Forms 2.0 specification. This specification is currently a mature working draft and has been adopted by the W3C HTML Working Group to serve as a starting point for the next version of HTML. This implementation will follow the HTML 5 specification that evolves from the W3C process.

The implementation has been tested and should function in:
  * Mozilla Firefox 1.0.8
  * Mozilla Firefox 1.5.0.9
  * Mozilla Firefox 2
  * Internet Explorer 6
  * Internet Explorer 7
  * Safari 2.0.4
  * Safari 3 (Windows)
  * Opera 9 _(native experimental implementation)_

## Implemented Features ##

  * **Extensions to form control elements**
    * The `form` and `select` elements are extended with `data` attributes for fetching values and options from external resources.
    * Extensions to the `input` element
      * Support for the new types `datetime`, `datetime-local`, `date`, `month`, `week`, `time`, `number`, `range`, `email`, and `url` (widgets not yet created for new controls; creating hooks into widgets from libraries such as Ext is the plan)
      * Ranges: the `min` and `max` attributes
      * Precision: the `step` attribute
    * Extensions to existing attributes
      * The `maxlength` attribute for `textarea` elements
    * The `pattern` attribute
    * The `required` attribute
    * The `autofocus` attribute
  * **The repetition model for repeating form controls**
    * New form controls: `add`, `remove`, `move-up`, and `move-down` buttons, along with the `template` attribute for `add` buttons
    * The `repeat-min` and `repeat-max` attributes
    * Event interface for repetition events
      * The `added`, `removed`,and `moved` events
      * _Extension:_ the `onadded`, `onremoved`, and `onmoved` HTML attributes and DOM properties
    * The repetition model
      * Addition
      * Removal
      * Movement of repetition blocks
      * Initial repetition blocks
  * **The forms event model:** Form validation
    * The `invalid` event and the `oninvalid` HTML attribute and DOM property
    * The `willValidate` DOM property
    * The `validity` interface
    * The `checkValidity` method on form controls and the `form` element
    * The `setCustomValidity` method on form controls
  * **Fetching data from external resources (with the `data` attribute)**
    * Filling `select` elements
    * Seeding a form with initial values

For more information on the implemented features, see the [implementation details](http://code.google.com/p/webforms2/wiki/ImplementationDetails).

## Usage ##
Load `webforms2.js` in the `head` of your document:
```
<script type='text/javascript' src='webforms2.js'></script>
```
Or you may include the compressed version of this file (via Dean Edwards' [packer](http://dean.edwards.name/packer/) along with other optimizations):
```
<script type='text/javascript' src='webforms2-p.js'></script>
```

It is important that the file `webforms2.css` and `webforms2-msie.js` both be located in the same directory as `webforms2.js` or `webforms2-p.js` (whichever you decide to use). For users of Internet Explorer, the file `webforms2-msie.js` is part of a [hack](http://dean.edwards.name/weblog/2005/09/busted/) that ensures that the Web Forms 2.0 functionality is initialized before the `load` event.

Alternatively, you may install a [user script](http://webforms2.googlecode.com/svn/trunk/webforms2.user.js) that enables testing (and usage) on any site.

Once the library has been included, then write HTML and JavaScript that take advantage of the features that Web Forms 2.0 specifies. In authoring XHTML pages using Web Forms 2.0, you may include the following DTDs to validate your code (these DTDs have passed validation but they have not been verified to correctly validate a WF2 document; please attempt to do so and write in with the results):
  * [XHTML 1.0 Strict + Web Forms 2.0 DTD](http://webforms2.googlecode.com/svn/trunk/DTD/xhtml1-strict-wf2.dtd)
  * [XHTML 1.0 Transitional + Web Forms 2.0 DTD](http://webforms2.googlecode.com/svn/trunk/DTD/xhtml1-transitional-wf2.dtd)

## Feedback ##
Either start a new discussion in this [implementation's group](http://groups.google.com/group/webforms2), send me your personal comments and suggestions via my [feedback form](http://weston.ruter.net/feedback.php?subject=WebForms2), or if you find a bug, you may [file an issue](http://code.google.com/p/webforms2/issues/list).

## Donations ##
If you value this software, [please donate](http://weston.ruter.net/donate/) to ensure that it may continue to be maintained and improved. Thank you!