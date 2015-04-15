# Random Account Naming in Javascript

Similar to Heroku, this is a simple implementation of a random name generator I use.

```javascript 
function {
    var adjs = [
        "autumn", "hidden", "bitter", "misty", "silent", "empty", "dry", "dark",
        "summer", "icy", "delicate", "quiet", "white", "cool", "spring", "winter",
        "patient", "twilight", "dawn", "crimson", "wispy", "weathered", "blue",
        "billowing", "broken", "cold", "damp", "falling", "frosty", "green",
        "long", "late", "lingering", "bold", "little", "morning", "muddy", "old",
        "red", "rough", "still", "small", "sparkling", "throbbing", "shy",
        "wandering", "withered", "wild", "black", "young", "holy", "solitary",
        "fragrant", "aged", "snowy", "proud", "floral", "restless", "divine",
        "polished", "ancient", "purple", "lively", "nameless"
    ];
    var nouns = [
        "waterfall", "river", "breeze", "moon", "rain", "wind", "sea", "morning",
        "snow", "lake", "sunset", "pine", "shadow", "leaf", "dawn", "glitter",
        "forest", "hill", "cloud", "meadow", "sun", "glade", "bird", "brook",
        "butterfly", "bush", "dew", "dust", "field", "fire", "flower", "firefly",
        "feather", "grass", "haze", "mountain", "night", "pond", "darkness",
        "snowflake", "silence", "sound", "sky", "shape", "surf", "thunder",
        "violet", "water", "wildflower", "wave", "water", "resonance", "sun",
        "wood", "dream", "cherry", "tree", "fog", "frost", "voice", "paper",
        "frog", "smoke", "star"
    ];
        var adj_index = Math.floor(Math.random() * adjs.length);
        var noun_index = Math.floor(Math.random() * nouns.length);
        var rnd = Math.floor(Math.random() * 9999);
    return adjs[adj_index] + "-" + nouns[noun_index] + "-" + rnd
}
// Example output:
//    icy-rain-7895
//    wandering-surf-3175
//    white-river-5801
//    muddy-resonance-846
```
There are 64 adjectives, 64 names and 9999 possible integers, for just under 41 million random combinations.

The longest adjective has 9 chars while the longest noun has 10 chars. The shortest adjective and noun has 3 chars. The integer varies from 1 char (0) to 4 chars (9999). Therefore, including the two "-" I ad, the returned name ranges from 9 to 25 characters in length.