# Detecting Browsers

### *Evergreen* Browsers
- IE10
- IE11+
- Chrome
- Firefox 
- Safari 7+
- Chrome Android 
- Mobile Safari
- (Opera?)

### Parsing the User-Agent String

This is the method used by [bowser.js](https://github.com/ded/bowser/blob/master/src/bowser.js).

**Internet Explorer**
``javascript
function getInternetExplorerVersion()
// Returns the version of Internet Explorer or a -1
// (indicating the use of another browser).
{
  var rv = -1; // Return value assumes failure.
  if (navigator.appName == 'Microsoft Internet Explorer')
  {
    var ua = navigator.userAgent;
    var re  = new RegExp("MSIE ([0-9]{1,}[\.0-9]{0,})");
    if (re.exec(ua) != null)
      rv = parseFloat( RegExp.$1 );
  }
  return rv;
}
function checkVersion(redirect_url)
{
  var msg = "You're not using Internet Explorer.";
  var ver = getInternetExplorerVersion();
  if ( ver > -1 )  {
    if ( ver >= 8.0 ) 
      window.location.href = redirect_url
  }
  alert( msg );
}
var redirect_url = "/support.html";
checkVersion(redirect_url);
``
Drawbacks

- Javascript must be enabled.
- The user-agent attribute is dynamic and can edited by the user, plugins, etc.

### Conditional Comments (IE only)

This is a script-disabled approach that relies on *Conditional Comment*, a feature _only supported by IE_.

``markup
<!--[if gte IE 8]>
<p>You're using a recent version of Internet Explorer.</p>
<![endif]-->
<!--[if lt IE 7]>
<p>Hm. You should upgrade your copy of Internet Explorer.</p>
<![endif]-->
<![if !IE]>
<p>You're not using Internet Explorer.</p>
<![endif]>
``

Drawbacks

- Only supported by IE.

### _Feature Detection_

This approach relies on testing the browser's functionality instead of it's version.

``javascript
if (XMLHttpRequest)
{
    // This browser implements this feature. 
}
``
----

## Resources

- [Detecting Windows Internet Explorer More Effectively - MSDN](https://msdn.microsoft.com/en-us/library/ms537509%28v=vs.85%29.aspx#UsingCCs)
- [bowser.js](https://github.com/ded/bowser)
- [Browser Detection Using The User Agent - MDN](https://developer.mozilla.org/en-US/docs/Browser_detection_using_the_user_agent)
- [Browser vs Feature Detection](http://jibbering.com/faq/notes/detect-browser/)