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

// destroy it if it is no longer needed
tokenizr.detroy(token); // back data based on token received before while still active
```

## Example

```javascript
const express = require("express");
const app = express();
const bodyParser = require("body-parser");
const tokenizr = require("@vinval/tokenizr");
const cookier = require("@vinval/cookier");

app.use(bodyParser.urlencoded({ extended: true }));

const users = {
    user1: {
        password: "12345"
    },
    user2: {
        password: "67890"
    },
}

const checkUser = (username, password) => {
    try {
        if (users[username].password == password) return users[username];
    } catch (error) {
        return false;
    }
    return false;
}

app.get("/", (req,res,next)=>{
    const cookies = cookier.parse(req.headers.cookie);
    const user = tokenizr.verify(cookies.token);
    if (user) {
        // do what you prefer with user object
    }
})

app.post("/login", (req,res,next)=>{
    const { password, username } = req.body;
    const user = checkUser(username, password);
    if (user) {
        const token = tokenizr.sign(user, 60**2 * 24 * 7); // 1 token duration 1 week
        // after that, use token how to prefer
    } else {
        // user not found
    }
})

app.listen(3000);
```
