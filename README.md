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

## Example

```javascript
const express = require("express");
const app = express();
const tokenizr = require("@vinval/tokenizr");
const cookier = require("@vinval/cookier");

const users = {
    user1: {
        password: "12345"
    },
    user2: {
        password: "67890"
    },
}

const checkUser = (username, password) => {
    return users[username] || false;
}

app.get("/", (req,res,next)=>{
    const cookies = cookier.parse(req.headers.cookie);
    const user = tokenizr.verify(cookies.token);
    if (user) {
        // do what you prefer with user object
    }
})

app.get("/login", (req,res,next)=>{
    const username = req.body.username;
    const password = req.body.password;
    const user = checkUser(username, password);
    if (user) {
        const token = tokenizr.sign(user, 60**2 * 24 * 7); // 1 token duration 1 week
        // after that, use token how to prefer
    } else {
        // user not found
    }
})
```
