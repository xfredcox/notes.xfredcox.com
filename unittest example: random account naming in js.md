# Unittest Example: Random Account Naming 

Example Mocha/Chai unit tests for our [Random Account Naming in Javascript].

```javascript
// Example output:
//    icy-rain-7895
//    wandering-surf-3175
//    white-river-5801
//    muddy-resonance-846
         suite('herokuNaming', function() {
             suiteSetup(function(){
                 this.NAMES = []
                 for (var i=0; i<10000; i++) {
                     this.NAMES.push(herokuNaming());
                 }
             });
             test('Type Test', function() {
                 for (var i in this.NAMES) {
                     assert.isString(this.NAMES[i]);
                     assert.isFalse(this.NAMES[i] == 'undefined');
                 }
             });
             test('Length Test', function() {
                 for (var i in this.NAMES) {
                     assert.isTrue(8 < this.NAMES[i].length < 26);
                 }
             });
             test('Inequality Test', function() {
                 // Test has a 1 in 40m chance of Type II error
                 for (var i = 0; i < this.NAMES.length - 1; i++) {
                     assert.isFalse(this.NAMES[i] == this.NAMES[i+1]);
                 }
             });
         })
```

You still need a uniqueness test that checks your existing accounts. I guess that's the cost of having **human readable** random account names.