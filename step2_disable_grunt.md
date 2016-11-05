# Disabling Grunt
Disabling grunt is a move that was advised to me, in Sails.js there are a few things you have to remember

## Disabling
Open the file tasks/.sailsrc

```json
{
  "hooks": {
    "grunt": false
  }
}
```

And then add access to your public assets

```json
{
  "paths": {
    "public": "assets"
  }
}
```
