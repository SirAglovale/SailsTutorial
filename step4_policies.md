# Policies
Policies in Sails are used for allowing access to routes
## Creating a policy
Open policy/hasSessionLogin.js
```javascript
modules.exports = function(req, res, next){
  if(req.session.username)
  {
    User.findOne({username: req.session.username}, function(err, user)
    {
      if(user) { return next(); }
      else { return res.badRequest("Not Authorised"); }
    }
  }
  else
  {
    return res.badRequest("Not Authorised");
  }
};
```

## Apply Policies
Open config/policies.js
```javascript
UserController: {
  create: true,
  login: true,
  get: ['hasSessionLogin']
}
'*': ['hasSessionLogin']
```
