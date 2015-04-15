# Description & Functionality 

The core-collapse element is a container element with two states: *opened* and *closed*. When *open*, the element displays the inner content. The toggle has a configurable transition effect.

``markup
<div class="heading" onclick="document.querySelector('#collapse1').toggle();">Collapse #1</div>
<core-collapse id="collapse1">
  <div class="content">
    <div>Lorem ipsum dolor sit amet.</div>
  </div>
</core-collapse>
``

# Implementation

The polymer implementation is quite simple, holding the element state in the *opened* attribute and offering toggle methods.

## Flow

el.toggle() >> this.opened = !this.opened >> this.openedChanged >> this.update() >> this.show() OR this.hide()

## Public Attributes

- **target**
  - The target element that will be opened when the core-collapse is opened. 
  - If unspecified, the core-collapse itself is the target.

``javascript
target: null,
ready: function() {
      this.target = this.target || this;
    },
``

- **horizontal**
  -  If true, the orientation is horizontal; otherwise is vertical.

``javascript
horizontalChanged: function() {
      this.dimension = this.horizontal ? 'width' : 'height';
    },
``

- **opened** 
  - Set opened to true to show the collapse element and to false to hide it.

``markup
toggle: function() {
      this.opened = !this.opened;
    },
``

- **duration** 
  - Collapsing/expanding animation duration in second.

- **fixedSize**  
  - If true, the size of the target element is fixed and is set on the element.  _Otherwise it will try to use auto to determine the natural size to use for collapsing/expanding_.
  - If this is not set, any target element dimension styling will be overridden.

- **allowOverflow**
  - By default the collapsible element is set to overflow hidden. This helps avoid element bleeding outside the region and provides consistent overflow style across opened and closed states. Set this property to true to allow the collapsible element to overflow when it's opened.
  - _TODO: Produce Example. Use is not quite clear._

## Events

- **core-collapse-open**
  - Fired when the core-collapse's *opened* property changes.

``javascript
openedChanged: function() {
      this.update();
      this.fire('core-collapse-open', this.opened);
    },
``

- **core-resize**
  - Fired when the target element has been resized _as a result of the opened state changing_.
  - So this seems like a core-resize-**complete** event really.

``javascript
transitionEnd: function() {
  if (this.opened && !this.fixedSize) {
      this.updateSize('auto', null);
  }
  this.setTransitionDuration(null);
  this.toggleOpenedStyle(this.opened);
  this.toggleClosedClass(!this.opened);
  this.asyncFire('core-resize', null, this.target);
  this.notifyResize();
},
``

# Gotchas

- core-collapse adjusts the height/width of the collapsible element to show/hide the content.  So avoid putting padding/margin/border on the collapsible directly, and instead put a div inside and style that.

# Topics of Interest

## Show & Hide Implementations

``javascript
    show: function() {
      this.toggleClosedClass(false);
      // for initial update, skip the expanding animation to optimize
      // performance e.g. skip calcSize
      if (!this.afterInitialUpdate) {
        this.transitionEnd();
        return;
      }
      if (!this.fixedSize) {
        this.updateSize('auto', null);
        var s = this.calcSize();
        if (s == '0px') {
          this.transitionEnd();
          return;
        }
        this.updateSize(0, null);
      }
      this.async(function() {
        this.updateSize(this.size || s, this.duration, true);
      });
    },
``

``javascript
    hide: function() {
      this.toggleOpenedStyle(false);
      // don't need to do anything if it's already hidden
      if (this.hasClosedClass && !this.fixedSize) {
        return;
      }
      if (this.fixedSize) {
        // save the size before hiding it
        this.size = this.getComputedSize();
      } else {
        this.updateSize(this.calcSize(), null);
      }
      this.async(function() {
        this.updateSize(0, this.duration);
      });
    }
``

_TODO: Annotate Code_

## Polymer.CoreResizer Mixin

> Any controls which may hide/show child elements, or that can resize child elements should moving forward pull in the Polymer.CoreResizer mixin, and call the notify resize method whenever it may change size. 

``javascript
Polymer('core-collapse', Polymer.mixin({
// ... core-collapse element code ...
}, Polymer.CoreResizer)
``

I hope to cover the [core-rezisable] element soon.

# Sources

- [Polymer Element Page](https://www.polymer-project.org/docs/elements/core-elements.html#core-collapse)
- [Source Code](https://github.com/Polymer/core-collapse)