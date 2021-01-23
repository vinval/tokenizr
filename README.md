# tokenizr
A simple node token auth system.

```javascript
const tokenizr = require("@vinval/tokenizr");

// sign({}, timeInSeconds or Date);

// back the token for every use and by default one day duration
const token = tokenizr.sign({userdata: {...}}); 

// or back the token for every use and 7 day duration
const token = tokenizr.sign({userdata: {...}}, 60 * 60 * 24 * 7);

// or back the token for every use and one day end
const token = tokenizr.sign({userdata: {...}}, new Date(year,month,day,hour,minutes,...));

// next, for would verify your token and callback your data
tokenizr.verify(token); // back data based on token received before while still active
```
