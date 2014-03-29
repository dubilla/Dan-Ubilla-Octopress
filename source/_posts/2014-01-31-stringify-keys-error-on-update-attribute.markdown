---
layout: post
title: "'stringify_keys' Error on update_attribute"
date: 2014-01-31 00:16
comments: true
categories: web-development
published: true
---
Rails is a fantastic framework to develop with, but it can occasionally be unforgiving when it comes to error throwing. I was recently coding up a soft delete method in a model when Rails gave me the perplexing error: "Undefined Method `stringify_keys'". I wasn't calling stringify_keys anywhere in my method nor was it anywhere in my model. A grep through the app directory of the codebase came up empty as well, and I was stumped.

<!-- more -->

As it turns out, the error was being thrown from deeper in the code than in my application, but it was, in fact, my code that was in error.

``` ruby "The update_attributes method in error"
def remove!
  self.update_attributes(:status, 'removed')
end
```

The error lies in the arguments that are being passed into update_attributes. update_attributes expects one argument, a hash, whereas, I am providing two arguments, a key and a value. The error is exactly four characters long. The corrected code is below.

``` ruby "The corrected update_attributes"
def remove!
  self.update_attributes(status => 'removed')
end
```

h/t to [Stack Overflow](http://stackoverflow.com/questions/7542774/undefined-method-stringify-keys-when-calling-update-attributes)
