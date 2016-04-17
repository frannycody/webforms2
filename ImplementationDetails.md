# Overview #

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
  * **Extensions to the HTML Level 2 DOM interfaces**
    * Additions specific to the `HTMLFormElement` interface
```
boolean checkValidity();
```
    * Additions specific to the `HTMLSelectElement` interface
```
         attribute boolean         autofocus;
readonly attribute boolean         willValidate;
readonly attribute boolean         willValidate;
readonly attribute DOMString       validationMessage;
boolean            checkValidity();
void               setCustomValidity(in DOMString error);
```
    * Changes to the `HTMLInputElement` interface
```
         attribute DOMString       min;
         attribute DOMString       max;
         attribute DOMString       step;
         attribute DOMString       pattern;
         attribute boolean         required;
         attribute boolean         autofocus;
readonly attribute boolean         willValidate;
readonly attribute ValidityState   validity;
readonly attribute DOMString       validationMessage;
boolean            checkValidity();
void               setCustomValidity(in DOMString error);
```
    * Additions specific to the `HTMLElement` interface
```
         attribute DOMString       pattern;
         attribute boolean         required;
         attribute boolean         autofocus;
         attribute long            maxLength;
readonly attribute boolean         willValidate;
readonly attribute ValidityState   validity;
readonly attribute DOMString       validationMessage;
boolean            checkValidity();
void               setCustomValidity(in DOMString error);
```
    * Additions specific to the `HTMLButtonElement` interface
```
readonly attribute RepetitionElement htmlTemplate;
```
    * The `RepetitionElement` interface
    * The `ValidityState` interface
    * Validation APIs
    * Repetition interfaces

_Details to be added for non-repetition features._

# Repetition Model #

The following sections correspond to sections in the [specification](http://whatwg.org/specs/web-forms/current-work/#repeatingFormControls).

## 3.3. New form controls ##
The new form controls may be created using either `button` or `input` elements; however, all buttons created with `input` elements will be dynamically replaced with `button` elements. It is recommended that authors stick to using `button` elements, as also the specification recommends. See [test case 9](http://weston.ruter.net/projects/repetition-model/testsuite/009.html).

When a button is clicked, the associated repetition behavior is invoked by its `click` handler. Buttons which have `onclick` handlers specified may cancel the default action by setting a `returnValue` to `false` on the button. Canceling the default action is only possible using the traditional event handler method of setting `onclick` property or HTML attribute (using `addEventListener` will not work). A technique which takes into account native implementations is to define a handler as follows:
```
removeButton.onclick = function(event){
    return this.returnValue = confirm("Are you sure you wish to remove this?");
}
```

## 3.5. Event interface for repetition events ##
Repetition events are not created of type `RepetitionEvent` but rather of type `UIEvents`. After the `UIEvents` is created, it is upgraded via the `RepetitionEvent._upgradeEvent` method. To dispatch new DOM Level 2 repetition events taking into account browsers which support the `RepetitionEvent` interface natively:
```
var addEvt;
try {
    addEvt = document.createEvent("RepetitionEvent");
}
catch(e){
    addEvt = document.createEvent("UIEvents");
    RepetitionEvent._upgradeEvent.apply(addEvt);
}
addEvt.initRepetitionEvent("added", true, false, repetitionBlock);
repetitionTemplate.dispatchEvent(addEvt);
```
The `initRepetitionEventNS` method is not implemented as mentioned above.

The specification's `added`, `removed`, and `moved` DOM Level 2 events are supplemented by the implementation of three events in the traditional model: `onadded`, `onremoved`, and `onmoved` respectively (in reversal of the previous state, `onadd`, `onremove`, and `onmove` are now deprecated). These three events may be registered either by setting HTML attributes or DOM properties of the same name on a repetition template. This implementation also makes these events available to browsers which implement the specification natively (i.e. Opera). Implementing the traditional event model for repetition events allows MSIE to handle the events fired by this implementation. The argument to handlers of these events is an `event` object, with the repetition block's DOM node set on this argument's `element` property (per the specification). Note that in previous versions of this implementation, the argument passed to these event handlers was not the object `event` but the DOM node `block` (which is now correctly located in `event.element`).

## 3.6. The repetition model ##
The repetition template is hidden via setting the element's `style` attribute to `display:none`.

The property `form.templateElements` is not implemented. Furthermore, form controls which would be included therein **are** incorrectly present in the forms' `elements` DOM attributes. This is due to the fact that NodeLists are read-only.

Form fields occurring within the repetition template are not 'successful' as specified; this is implemented by means of setting all of their `disabled` attributes. In order for authors to disable form fields within the template element so that when they are cloned they remain disabled in the blocks, they must also include within their `class` attributes the value "disabled". This is due to a Firefox issue, documented in issue [#9](http://code.google.com/p/repetitionmodel/issues/detail?id=9). For example:
```
  <input type="text" name="field[i]" disabled="disabled" class="disabled" />
```

### 3.6.1. Addition ###
Steps 9 and 10 of the algorithm are inserted after step 4; this has no bearing on the validity of the implementation.

While step 11 states that a delimited repetition template ID in every attribute on the new block must replaced by the new repetition block's index, this does not work in Firefox and MSIE in attributes which are not defined as CDATA (such as `border`, although it works to a limited extent in `style` when part of a URL).

In addition to an `added` event being fired on the repetition template, if the repetition template has an `onadded` DOM property or HTML attribute which contains a function, it will also be called with an `event` argument. This is an [extension](http://code.google.com/p/repetitionmodel/wiki/Extensions) to the specification which was included in order to allow Internet Explorer to handle repetition events.

The automatic disabling of the `add` buttons **does** affect the DOM `disabled` attribute. It is **not** an intrinsic property of these buttons.

### 3.6.2. Removal ###
In addition to a `removed` event being fired on the repetition template, if the repetition template has an `onremoved` DOM property or HTML attribute which contains a function, it will also be called with an `event` argument. This is an [extension](http://code.google.com/p/repetitionmodel/wiki/Extensions) to the specification which was included in order to allow Internet Explorer to handle repetition events.

The automatic disabling of the `remove` buttons **does** affect the DOM `disabled` attribute. It is **not** an intrinsic property of these buttons.

### 3.6.3. Movement of repetition blocks ###
In addition to a `moved` event being fired on the repetition template, if the repetition template has an `onmoved` DOM property or HTML attribute which contains a function, it will also be called with an `event` argument. This is an [extension](http://code.google.com/p/repetitionmodel/wiki/Extensions) to the specification which was included in order to allow Internet Explorer to handle repetition events.

The automatic disabling of the `move-up` and `move-down` buttons **does** affect the DOM `disabled` attribute. It is **not** an intrinsic property of these buttons.

## 7. Extensions to the HTML Level 2 DOM interfaces ##
All attributes which are defined as `readonly` are not, but they should be treated as such. In the future, the getters and setters of JavaScript 1.5 may be employed to emulate this readonly behavior.


```
interface HTMLFormElement : HTMLElement {
  ...
  readonly attribute HTMLCollection  templateElements;
  ...
};
interface HTMLButtonElement : HTMLElement {
  ...
  readonly attribute RepetitionElement htmlTemplate;
  ...
};
interface RepetitionElement {
  const              unsigned short  REPETITION_NONE = 0;
  const              unsigned short  REPETITION_TEMPLATE = 1;
  const              unsigned short  REPETITION_BLOCK = 2;

           attribute unsigned short  repetitionType;
           attribute long            repetitionIndex;
  readonly attribute Element         repetitionTemplate;
  readonly attribute HTMLCollection  repetitionBlocks;
           attribute unsigned long   repeatStart;
           attribute unsigned long   repeatMin;
           attribute unsigned long   repeatMax;
  Element            addRepetitionBlock(in Node refNode);
  Element            addRepetitionBlockByIndex(in Node refNode, in long index);
  void               moveRepetitionBlock(in long distance);
  void               removeRepetitionBlock();
};
```

### 7.13. Repetition interfaces ###
_The repetition interfaces are implemented without the use of bindings, getters, and setters._ For this reason, the implementation is not completely valid. Developers using this implementation should:
  1. Treat all `readonly` properties as such.
  1. Eliminate the setting DOM properties to which this specification assigns side-effects (i.e., `repetitionType`).
  1. Choose to only set DOM properties (e.g., `repetitionIndex`) when a corresponding HTML attribute (e.g., `repeat`) is also available.
  1. If dynamically creating new add, remove, or move buttons, call the appropriate function below after it has been inserted into the document (you may supply another element besides `document` if you only wish to update a portion of the document):
```
RepetitionElement._init_repetitionButtons('add');
RepetitionElement._init_repetitionButtons('remove');
RepetitionElement._init_repetitionButtons('move-up');
RepetitionElement._init_repetitionButtons('move-down');
```

The RepetitionElement interface is implemented by all elements in browsers which support modification of `Element.prototype`. In the future, DHTML Behaviors may make this possible for MSIE as well. In the mean time, checking the condition `!el.repetitionType` should be used instead of `el.repetitionType == RepetitionElement.REPETITION_NONE`.

Setting `repetitionType` does not modify the `repeat` attribute; this will require the use of  setter. Thus if `repetitionType` is set to `REPETITION_NONE`, the attribute will not be removed. Likewise, if it is set to `REPETITION_TEMPLATE`, the attribute is not modified (not set to "`template`"). And if `repetitionType` is set to `REPETITION_BLOCK`, no change is made to the `repeat` content attribute when it should be set to the value of the `repetitionIndex` DOM attribute. No DOM exception is raised if it is set to any other value.

If the element is a repetition block, setting the `repetitionIndex` DOM property does **not** update the `repeat` attribute appropriately (**nor** does changing the attribute directly  affect the value of the `repetitionIndex` attribute). If the element is a normal element, it will return undefined or `REPETITION_NONE` (zero), but setting the attribute **does** incorrectly work (due to the lack of setters).

The `repetitionTemplate` attribute is null (or **undefined**) unless the element is a repetition block, in which case it points to the block's template.

The `repetitionBlocks` attribute is null (or **undefined**) unless the element is a repetition template, in which case it points to a list of elements, implemented not as an `HTMLCollection`, but as an `Array` object. Therefore, this list is **not** live. For this reason, repetition blocks should only be added to, moved in, or removed from a template by means of the RepetitionElement interface methods: `addRepetitionBlock(ByIndex)`, `moveRepetitionBlock`, and `removeRepetitionBlock`. These methods ensure that the `repetitionBlocks` list currently reflects the actual DOM tree. This implementation deficiency may be remedied in the future by listening for mutation events.

The `repeatStart`, `repeatMin`, and `repeatMax` DOM properties do reflect the values of the `repeat-start`, `repeat-min`, and `repeat-max` HTML attributes respectively, but only when the repetition template is first initialized. Changing the HTML attributes thereafter will have **no affect** on the repetition template's behavior. Setting the DOM properties **does not affect** the relevant content (HTML) attributes.

The `htmlTemplate` DOM attribute on the `HTMLButtonElement` (**not** `HTMLInputElement`) interface represents the repetition template that the template content attribute refers to. This DOM attribute is **not** readonly, but it should be treated as such.
