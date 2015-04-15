# CSS Positioning - Pure CSS

## Position Attribute

HTML element positions are set by the left / right / top / bottom attribute, though these have different effect depending on the position method of the element. There are four possible position methods:

**Static**

  - Default value;
  - Element follows the natural flow;
  - Unaffected by the left/right/top/bottom;

**Fixed**

  - Fixed relative to the browser window;
  - Other elements act as if the element were not there (hence, liable to overlap);

**Relative**  

  - Relative to the element's otherwise static position;
  - Relative elements can overlap, but their space is preserved in the flow of elements;

**Absolute**

  - Positioned relative to the first parent with a position method other than static - or <html> if none;
  - Other elements don't recognise absolute positioned elements;

## Float Attribute

The float attribute causes an element to detach from its parent. It's position still depends on the parents' and on other floating siblings, but the the parent's shape is no longer affected by the children - leading to a height of 0px. This is a problem if you need to implement visible attribute on the container element (ie. background colors). You can overcome this by giving the parent element an overflow attribute value of auto or by implementing a clearfix. 

Clearfixes can be set using :before/:after pseudo elements or a .clearfix element. 

``markup
<!--  :after pseudo element method -->
<style>
.clearfix:after { 
   content: "."; 
   visibility: hidden; 
   display: block; 
   height: 0; 
   clear: both;
}
</style>
<!--  .clearfix method-->
<div style="clear: both;"></div>
``

----
Source:

[W3C CSS Positioning](http://www.w3schools.com/css/css_positioning.asp)

[Detailed CSS Positioning - Shay Howe](http://learn.shayhowe.com/advanced-html-css/detailed-css-positioning/)

[All About Floats - CSS Tricks](https://css-tricks.com/all-about-floats/)