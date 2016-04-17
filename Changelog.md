## 0.5.5 (future) ##
  * Add support for the `valueAsDate` and `valueAsNumber` DOM attributes, as well as the `stepUp` and `stepDown` methods.
  * Add (and complete) support for DOM attributes, such as `repetitionType`, `pattern`, `required`, `autofocus`, `min`, `max`, and `step`. Add getters and setters for these when such is available so that setting these attributes may have side effects.
  * Add Mutation Event handler for `DOMAttrModified` events so that the modification of content attributes (such as `repeat`) may result in side effects (such as constructing a new repetition template).
  * Ensure that the DOM element prototypes are extended (instead of each individual instance) whenever possible (such as setting `HTMLFormElement.prototype.checkValidity`)
  * Enable support for XHTML documents and support for XML namespaces.

## 0.5.4 (2008-07-29) ##
  * Fixed an issue where using setCustomValidity() was not persisting the customError validity state to when the control or form's validity was checked.

## 0.5.3 (2008-07-28) ##
  * Fixed an issue where a form control that was dynamically disabled after page load would would still have willValidate == true

## 0.5.2 (2007-11-29) ##
  * Added fix for MSIE so that `option` elements with the `selected` attribute will be correctly cloned in repetition blocks (see updated [test case 18](http://webforms2.googlecode.com/svn/trunk/testsuite/018.html)).

## 0.5.1 (2007-09-22) ##
  * Corrected the capitalization of the DOM attribute "`maxLength`".
  * Fixed [issue 6](https://code.google.com/p/webforms2/issues/detail?id=6) by adding null argument to XMLHttpRequest.send() since Firefox requires it; issue was hidden by Firebug 1.0.
  * Fixed DOMException emulation for MSIE.
  * Changed the pack.pl so that Packer doesn't base64 encode the source into webforms2-p.js as configuring the server to gzip is a better solution. This file has the source code with all comments and whitespace removed, and all internal identifiers are replaced with short codes.
  * Provided a [user script](http://webforms2.googlecode.com/svn/trunk/webforms2.user.js) that enables testing (and usage) on any site.
  * Added a couple test cases to the [test suite](http://webforms2.googlecode.com/svn/trunk/testsuite/index.html).

## 0.5 (2007-08-14) ##
_Initial release_
  * Extensions to the `input` element
    * New types: `datetime`, `datetime-local`, `date`, `month`, `week`, `time`, `number`, `range`, `email`, and `url` (widgets not yet created for new controls)
    * The `min`, `max`, and `step` attributes (not yet implemented for `file` inputs)
    * The `pattern` attribute
  * The `maxlength` attribute for `textarea` elements.
  * The `required` and `autofocus` attributes
  * The repetition model for repeating form controls and the repetition interfaces (incorporated from the existing repetition model [implementation](http://code.google.com/p/repetitionmodel/))
  * Form validation model
  * Fetching data from external resources: the `data` attribute for `select` and `form` elements

# Repetition Model #
The following log refers to releases of the standalone repetition model [implementation](http://code.google.com/p/repetitionmodel/) which has been incorporated into this Web Forms 2.0 project.

## 0.8.4+ (future) ##
  1. Fix MSIE memory leaks.
  1. Fix the populating of `forms[n].templateElements`.
  1. Try to enable support for bfcache so that the repetition blocks are not erased when the page is re-shone. See [Using Firefox 1.5 caching](http://developer.mozilla.org/en/docs/Using_Firefox_1.5_caching).

## 0.8.3.1 (upcoming in Web Forms 2.0 implementation) ##
  1. This WF2 Repetition Model implementation is being superseded by a full [Web Forms 2.0 implementation](http://code.google.com/p/webforms2/). _The code appearing in this project is now deprecated._
  1. Moved firing of `moved` event to after all of the movement code is fired.
  1. Made call to `window.onload` for initialization a last resort.
  1. Fixed number of repetition blocks added to be set to the value of `repeat-start`, not subtracting the number of hard-coded blocks. Error found through [move-down simple test (part two)](http://tc.labs.opera.com/html/repetition/movement/003.htm).
  1. Fixed bug where MSIE was not correctly converting repetition buttons specified with INPUT elements into BUTTON elements.
  1. Prevented user-supplied repetition event handlers from aborting the execution of the repetition behavior by triggering runtime errors.
  1. Moved custom `cloneNode` algorithm out of the `addRepetitionBlock` subroutine.
  1. Removed the globally applied Gecko JS 1.6 implementation of `Array.some`.
  1. New Greasemonkey script to enable behavior on any site.
  1. Enabled the script to fire immediately if it determines that the DOM has already been loaded. This enables including the script right before the closing `body` tag and it enables including the script via Greasemonkey.

## 0.8.3 (2007-05-16) ##
  1. Reduced file size of by ~12% by consolidating constructors and handlers for repetition buttons so that the code would not be repeated, and by reducing the amount of code needed to implement the extension for defining repetition event handlers by HTML attributes.
  1. Removed faulty implementation of `form.templateElements`.
  1. Added default labels for repetition buttons when the user provides none.
  1. Enabled the option to use INPUT elements as repetition buttons by substituting them for BUTTON elements.
  1. Improved handling of onclick handlers on repetition buttons.
  1. Added XPath support for DOM queries in supporting browsers.
  1. Removed `__getElementsByProperty`, and replaced call to it with a call to the XPath enabled `__getElementsByNameAndAttribute`.
  1. Checked for memory leaks in MSIE and found two; however, I was not able to remove them.
  1. Updated [test case 5](http://weston.ruter.net/projects/repetition-model/testsuite/005.html) to show contents of style attribute when replacement is successful.
  1. In [test case 9](http://weston.ruter.net/projects/repetition-model/testsuite/009.html), added repetition block indexes to each row to assist in keeping track of the changes.

## 0.8.2.2 (2007-03-28) ##
  1. Improved handling of `style` attribute so that a repetition index appearing within a CSS `background-image` property will be correctly replaced with the correct value. For example:
```
style='background-image:url(image[tmpl].png)'
                                 \/
style='background-image:url(image1.png)'
```
  1. Added [test case 13](http://weston.ruter.net/projects/repetition-model/testsuite/013.html) to verify this improvement and also to demonstrate repetition model-driven content in nested repetition templates.

## 0.8.2.1 (2007-03-27) ##
  1. Fixed minor logic bug where `removed` events were being fired before the removed block had been removed from `repetitionBlocks`.
  1. Added [test case 12](http://weston.ruter.net/projects/repetition-model/testsuite/012.html) to verify the previous fix.

## 0.8.2 (2007-03-23) ##
  1. Fixed the ability to cancel the default action of a repetition behavior. If a user sets  the `onclick` handler of a repetition button (`add`, `remove`, `move-up`, `move-down`) via either an HTML attribute or a DOM property, and if this handler returns false, then the button's respective behavior will not be executed. Since the `onclick` handler may be fired before the repetition behavior is able get the handler's return value, an ad hoc `returnValue` property should be set on the button in question. Assigning DOM Level 2 Event handlers or MSIE Event handlers which cancel the default action are not yet supported.
  1. Updated [test case 4](http://weston.ruter.net/projects/repetition-model/testsuite/004.html) to demonstrate and test this functionality.

## 0.8.1 (2007-01-24) ##
  1. Fixed `moveRepetitionBlock` algorithm so that `move-up` and `move-down` buttons are disabled if the repetition block could not be moved any higher according to **the algorithm**.
  1. Fixed step 5 of `moveRepetitionBlock` algorithm so that `insertBefore` is called on the **current block's** parent node, not on the current target.
  1. Fixed MSIE error raised when using [Prototype](http://www.prototypejs.org/) with Repetition Model (added check to see if `Element.prototype` was defined as well as `window.Element`)
  1. Revised the subroutine that enables and disables the movement buttons. This subroutine makes use of `Array.some` defined by JavaScript 1.5; if it does not exist, then the function is added to the `Array` prototype.
  1. Replaced `cssQuery` with smaller and lighter-weight specialized DOM query functions.
  1. Disabled Packer's encoding option in the creation of the the packed version. The encoding algorithm somehow caused problems with MSIE's `onmove` handler.
  1. Ensured that template form controls are not successful. When initializing, all controls within the template are disabled and then subsequently enabled when the template is cloned into a block.
  1. Allowed authors to disable form fields within the template element so that when they are cloned they remain disabled in the blocks. Note: due to a Firefox issue, documented in [issue #9](http://code.google.com/p/repetitionmodel/issues/detail?id=9), authors must include a class name of "disabled" on all form fields in addition to setting their `disabled` attribute. For example:
```
  <input type="text" name="field[i]" disabled="disabled" class="disabled" />
```

## 0.8 (2007-01-20) ##
  1. Streamlined `__createElementWithName` with code from [Anthony Lieuallen](http://www.easy-reader.net/archives/2005/09/02/death-to-bad-dom-implementations/#comment-444)
  1. Renamed methods on `RepetitionElement` interface (removed erroneous underscore from names)
  1. Added semicolons to all inline functions so that Packer would correctly function
  1. Added HTML attribute event handler support for Opera
  1. _New issue: Discovered that style attributes must be processed specially._ See [issue #7](http://code.google.com/p/repetitionmodel/issues/detail?id=7).
  1. The use of an external file to emulate `DOMContentLoaded` has been reinstated. See [issue #9](http://code.google.com/p/repetitionmodel/issues/detail?id=3).
  1. Focus now remains on a move button when it is clicked and its containing block is moved.
  1. Tested in Firefox versions 1.0.8 and 1.5.0.9
  1. Made fixes for Firefox 1.0 (including fixing typos to the DOM Level 2 event fallbacks)
  1. Updated `repetition-mode-wrapper.js` so that it would function in MSIE (requires `document.write`)

## 0.8 beta (2007-01-12) ##
  1. Migrated code hosting from SourceForge over to Google Code.
  1. Implemented the traditional event model for browsers which natively support the repetition model and which support `addEventListener` (i.e. Opera). This permits Ben Nolan's [Behaviour](http://bennolan.com/behaviour/) library to be used with the events fired by this repetition model implementation: repetition model events may now be handled by browsers which do not support `addEventListener` (i.e. MSIE); to do so, one may set `onadd`, `onremove`, `onmove` properties of a repetition template element, or one may set the HTML attributes of the same names on a repetition template's opening tag.
  1. Included compressed version (repetition-model-p.js) of the main script via Dean Edward's Packer.
  1. ~~For the MSIE-substitute for `DOMContentLoaded`, eliminated the need for separate code while under a HTTPS connection and thus eliminating the need for MSIE's deferred `repetition-model-init.js`. Removing `document.write` from being used in the substitute code apparently resolved the issue.~~ The use of an external file to emulate `DOMContentLoaded` has been reinstated. See [issue #9](http://code.google.com/p/repetitionmodel/issues/detail?id=3).
  1. Allowed `onclick` handlers on the repetition model buttons. Accomplished by using `addEventListener`/`attachEvent` instead of setting the `onclick` property.
  1. Enabled the possibility of preventing the default action of a repetition control button of being carried out. To do so, a button's `onclick` handler (will not work using `addEventListener` or `attachEvent`) must not just return `false` but must set a `returnValue` property on the button. So when desiring to cancel the default action, one may terminate an onclick handler with: `return this.returnValue = false`.
  1. Implemented requirement that "Repetition blocks without a `repeat-template` attribute are associated with their first following sibling that is a repetition template, if there is one."
  1. Implemented requirement that `remove`, `move-up`, and `move-down` buttons be automatically disabled if they are not in a repetition block.
  1. Implemented `RepetitionElement` interface on all elements in browsers which provide access to the DOM `Element` object; this also should improve performance since the repetition template's methods no longer need to be copied onto each instance. Hopefully this functionality, including a universal application of the `RepetitionElement`, will be possible in MSIE by means of behaviors. See [issue #2](http://code.google.com/p/repetitionmodel/issues/detail?id=2).
  1. Exceptions are thrown if repetition template methods are called on non-repetition template elements, and when the `refNode` argument to `addRepetitionBlock[ByIndex]` is not a DOM node.
  1. Worked on implementing the RepetitionEvent interface. See [issue #1](http://code.google.com/p/repetitionmodel/issues/detail?id=1).
  1. Fixed the dispatching of the `moved` event.
  1. Changed the order of operations in the `removeRepetitionBlock` algorithm so that the `removed` event is fired before the `added` event when the minimum number of blocks (`repeatMin`) is passed.
  1. Fixed `addRepetitionBlock[ByIndex]` which when passed `refNode`, would insert a block before `refNode` and not after as mandated by the spec.
  1. Fixed the cloning algorithm so that when new repetition block is added, the event handler attributes which are set on and within the repetition template are successfully cloned onto each block (this was not working in MSIE).
  1. An issue arose in Internet Explorer with its substitute for `DOMContentLoaded`, which when called initializes the repetition model before `onload` as required by the specification. The issue was this: when there are no external files to load, this substitute for `DOMContentLoaded` may unfortunately be erroneously fired after the document's onload handler. This problem was resolved when `document.write` was removed from the implementation of the hack, and replaced with `document.createElement` and `element.appendNode`. However, another issue arose regarding this `DOMContentLoaded` substitute: sometimes cssQuery does not find all of the repetition templates when called therein. This issue was also satisfactory resolved (pending a better solution utilize MSIE's behaviors, see [issue #3](http://code.google.com/p/repetitionmodel/issues/detail?id=3) and [issue #4](http://code.google.com/p/repetitionmodel/issues/detail?id=4)).
  1. Aligned the implementation with the specification where `addRepetitionBlock` returns `null` if `maxRepeat` is met.
  1. Removed the need in MSIE for the `removed`/`move-up`/`move-down` button constructors to be called more than once (the `_initialized` property was being cloned).
  1. Changed first argument for `onadd`, `onremove`, and `onmove` event handlers from single repetition block element to a regular `event` object constructed via `document.createEvent/createEventObject`.
  1. Fixed problem where `add` buttons were not being disabled when `repeatStart` was equal to `repeatMax` on page load; this happened because `addRepetitionBlock` was being called before the `add` buttons were constructed. Created a `__updateAddButtons` method to go along with the `__updateMoveButtons`.
  1. Changed the argument of `document.createEvent` from "UIEvent" to "UIEvents".

## 0.7.3 (2006-11-03) ##
  * _Initial public release._
  * External path changes in repetition-model-wrapper.js.
  * Disabled firing of events within the MSIE event model: fireEvent('onadd') throws an exception.
  * Added code for firing moved events.
  * Various fixes and improvements to example 1.

## 0.7.2 (2006-09-13) ##
  * _Internal stable release._





