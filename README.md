# nodemon infinite loop

If you are seeing a log like this...

```bash
$ npm start

> nodemon-test@1.0.0 start /Users/jinjor/Projects/nodemon-test
> nodemon --watch dir dir/index.js

[nodemon] 1.18.3
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: dir
[nodemon] starting `nodemon --watch dir dir/index.js dir/index.js`
[nodemon] 1.18.3
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: dir
[nodemon] starting `nodemon --watch dir dir/index.js dir/index.js dir/index.js`
[nodemon] 1.18.3
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: dir
[nodemon] starting `nodemon --watch dir dir/index.js dir/index.js dir/index.js dir/index.js`
[nodemon] 1.18.3
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: dir
[nodemon] starting `nodemon --watch dir dir/index.js dir/index.js dir/index.js dir/index.js dir/index.js`
```

you probably call nodemon in `start` in package.json!

```json
"scripts": {
  "start": "nodemon --watch dir dir/index.js"
},
```

If `dir/index.js` exists, it works as you expected.
If not, the command behaves like `nodemon --watch dir npm start -- dir/index.js`. Recursively calling `npm start` results in the infinite loop!

So maybe you want to generate `dir/index.js"` by using tsc, webpack or something, but no file is there at first. So, let's make it!

```bash
"scripts": {
  "prestart": "mkdir -p dir && touch dir/index.js",
  "start": "nodemon --watch dir dir/index.js"
},
```

Please correct me if something is wrong.


## Ref

- https://github.com/remy/nodemon/issues/933
- https://github.com/remy/nodemon/issues/996
- https://stackoverflow.com/questions/42318030/nodemon-recursive-watch-issue
- 
