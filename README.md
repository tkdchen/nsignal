# nsignal

nsignal works with Models defined by developers from mongoose' model. nsignal
gives developer the ability to run some functions when related object of Model
has change, like created, updated, or deleted.

# Usage

Define your signal by initialzing an object from Signal class.

    var Signal = require("nsignal");
    var post_save = new Signal({providing_args: ["instance"]});

Register signal receivers in your proper module.

    var SomeModel = require("/path/to/model");

    post_save.connect(SomeModel, function(sender, args, done) {
        console.log("Handle signal here");
        done(null, true);
    });

    // Other receivers
    // ...

``done`` must be called at each exit point within the receiver function.
Function done follows Node callback convention, 

Send signal at some point.

    post_save.send(savedObject, {"instance": savedObject});

where savedObject is the object of SomeModel and just be saved. As an example,
the args passes instance only, which has the same value with the first argument
of method send. Of course, you can pass any arguments through the second
argument, as long as they satisfy the arguments specified while defining
``post_save.``

If you want to handle the result, pass a callback function as the last argument.

    post_save.send(savedObject, {"instance": savedObject}, function(err, results) {
        console.log(">>>", results);
    });

where argument err will be always null, and results is an object of Array
containing each receiver's result and potential error object.
