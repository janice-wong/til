#cURL it, cURL it good

```
$ curl -s https://meme-api.herokuapp.com/gimme | jq
```

Here, we made an API call to [this meme generator](https://meme-api.herokuapp.com/gimme).

`-s` silences the request and hides any progress data.
` | jq` pretty prints the response.
