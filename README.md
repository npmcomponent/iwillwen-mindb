*This repository is a mirror of the [component](http://component.io) module [iwillwen/mindb](http://github.com/iwillwen/mindb). It has been modified to work with NPM+Browserify. You can install it using the command `npm install npmcomponent/iwillwen-mindb`. Please do not open issues or send pull requests against this repo. If you have issues with this repo, report it to [npmcomponent](https://github.com/airportyh/npmcomponent).*
![MinDB](http://iwillwen.u.qiniudn.com/min/mindb_logo.png?123)**Database on JavaScript**

Storing and structuring your application data on JavaScript.  
Providing a standard *Store Interface* and **Redis**-copy API that you can use in wherever.

[Chinese version](https://github.com/iwillwen/mindb/blob/master/README_zhcn.md)

# Installation

Loading via script tag:

    <script style="text/javascript" src="/path/to/script/min.js">

With [node](http://nodejs.org) previously installed:

    $ npm install min

If you  using [SeaJS](http://seajs.org), you can install with [spm](https://github.com/spmjs/spm2):

    $ spm install iwillwen/min

If you are using [component](http://component.io), you can install it with:

    $ component install iwillwen/mindb

Or [bower](http://bower.io/)?:

    $ bower install mindb
    
# Basic Usage

Common key-value via such as `SET`, `GET`, etc.

    min.set('foo', 'bar', function(err) {
      if (err) {
        return console.error(err);
      }
      
      min.get('foo', function(err, value) {
        if (err) {
          return console.error(err);
        }
        
        console.log(value); //=> bar
      });
    });

## Basic commands
- `set` Set the value of a key `(key, value[, callback])`
- `setnx` Set the value of a key, only if the key does not exist `(key, value[, callback])`
- `setex` Set the value and expiration of a key `(key, seconds, value[, callback])`
- `psetex` Set the value and expiration in milliseconds of a key `(key, millseconds, value[, callback])`
- `mset` Set multiple keys to multiple values `(plainObject[, callback])`
- `msetnx` Set multiple keys to multiple values, only if none of the keys exist `(plainObject[, callback])`
- `append` Append a value to a key `(key, value[, callback])`
- `get` Get the value of a key `(key[, callback])`
- `mget` Get the values of a set of keys `(keys[, callback])`
- `getset` Set the value of a key and return its old value `(key, value[, callback])`
- `strlen` Get the length of a key `(key[, callback])`
- `incr` Increment the integer value of a key by one `(key[, callback])`
- `incrby` Increment the integer value of a key by the given amount `(key, increment[, callback])`
- `incrbyfloat` Increment the float value of a key by the given amount `(key, increment[, callback])`

## Hash, List, Set, Sorted Set
Maybe you can get the way by browsing [Redis Commands](http://redis.io/commands). XD

## Sweet
Nested Callbacks? Maybe you would prefer [Promise](http://promises-aplus.github.io/promises-spec/):

    min.incr('user_id')
      .then(function(curr) {
        return min.hmset('user-' + curr, {
          name: 'Will Wen Gunn',
          id: 'iwillwen',
          email: 'willwengunn@gmail.com'
        });
      })
      .then(function(key) {
        var id = key.substr(5);
        
        return min.sadd('user-msg-' + id, 'WelCome!');
      })
      .then(function(length) {
        // ...
      })
      .fail(function(err) {
        console.log(err);
      });

Anymore else? How about `MULTI`?

    min.multi()
      .incr('msg-seq')
      .incr('msg-seq')
      .incr('msg-seq')
      .exec(function(err, results) {
        if (err) {
          return console.error(err);
        }
        
        console.log(results); //=> [ [ 1 ], [ 2 ], [ 3 ] ]
      });

Support multiple databases:

    var Min = min.fork();
    Min.set('foo', 'bar')
      .then(/*...*/)
      .fail(/*...*/);

# Store Interface
Read the [Store Interface Documentation](https://github.com/iwillwen/mindb/blob/master/docs/store_interface.md).

# Contributing
Contribution is welcome.There are more than one way to contribute, and I will appreciate any way you choose.

- tell your friends about iwillwen/mindb, let MinDB to be known
- discuss MinDB, and submit bugs with github issues
- send patch with github pull request
- donate MinDB

## Donate us
[![Donate with Alipay](http://iwillwen.u.qiniudn.com/donate-with-alipay.png)](http://me.alipay.com/iwillwen)

## git-flow
We recommend you to use [`git-flow`](https://github.com/nvie/gitflow) to make a patch.

Hint:

    $ git flow feature start [featurename]
    $ git add .
    $ git commit -m 'new feature description'
    $ git flow feature finish [featurename]

# License

Copyright (c) 2012-2013 Will Wen Gunn(willwengunn@gmail.com)
All rights reserved.

MIT License

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.
