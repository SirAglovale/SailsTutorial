# Adding users
## Creating the user API
```bash
sails generate api user
```
This will create 2 files, one called User.js in the models directory and another UserController.js in the Controllers directory

## Creating the user model
Open the file User.js
```javascript
//This is a basic user, you can add to it with passwords
module.exports = {
  _id: { 
    type: "string",
    primaryKey: true
  },
  username: {
    type: "string",
    unique: true
  }
};
```

## Creating the user controller
Open the file UserController.js
```javascript
//This will allow for creation and getting of users, A full description of all database operations can be found on Sailsjs.org
module.exports = {
  create: function(req, res)
  {
    User.findOne({username: req.body.username}, function(err, user) {
      if(err)
      {
        return res.badRequest(err);
      }
      if(user)
      {
        return res.badRequest("User Found.");
      }
      User.create({username: req.body.username, _id: uuid()}, function(err, user) {
        if(err)
        {
          return res.badRequest(err);
        }
        if(user)
        {
          user.save();
          return res.json(user);
        }
        return res.badRequest("Cannot create user.");
      });
    });
  },
  login: function(req, res)
  {
    User.findOne({username: req.body.username}, function(err, user) {
      if(err)
      {
        return res.badRequest(err);
      }
      if(user)
      {
        res.session.loggedin = user.username;
        return res.json(user);
      }
      return res.badRequest("Could not find user");
    });
  },
  get: function(req, res)
  {
    User.find({}, function(err, users) {
      if(err)
      {
        return res.badRequest(err);
      }
      if(users)
      {
        return res.json(users);
      }
      return res.badRequest("Could not find any users");
    });
  }
};

function uuid()
{
  function s4()
  {
    return Math.floor((1 + Math.random()) * 0x10000).toString(16).substring(1);
  }
  return s4() + "-" + s4() + s4();
}
