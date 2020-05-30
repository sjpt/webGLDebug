# webGLDebug
## summary
 __Gldebug__ implements a mechanism for wrapping and monitoring all webGL calls
 * __Gldebug.start(opts)__ applies a wrapper to all gl and gl extension functions.
 * __Gldebug.stop()__ restores the original gl builtin functions.

Gldebug imposes a very significant overhead so should be used sparingly,
but can be very helpful for debugging without the need to make code changes.
Calls to Gldebug can be made from code, or from Dev Tools.
One useful pattern is to add code that allows a clause in __location.search__ to start Gldebug.

The wrapper can check for gl errors before (should be unnecessary) and after every gl call.
The wrapper logs some statistics, and takes optional actions, depending on 'action' and any error found.

Gldebug has been coded for use with three.js, but should work for any WebGL application.

## details
 __action__ is a string that can contain one or more of
 *  __logall:__         all gl calls are logged
 *  __logerr:__         gl error calls are logged
 *  __breakall:__       debugger break on every gl call
 *  __breakerr:__       debugger break on error gl calls
 *  __checkbefore:__    checks are carried out before each call (by default they are not)
 *  __nocheckafter:__   checks are not carried out after each call (by default they are)

__Gldebug.start()__ takes an options object as input, which can contain
 *  __gl:__         gl context, if not given this may be deduced from window.gl or Gldebug.gl
 *  __action:__     action string as above: defaults to 'logerr'
 *  __frames:__     number of frames to debug before automatic stop: defaults to Infinity.
 *  __frameOwner:__ object in which the frame counter lives: defaults to window
 *  __frameName:__  name of frame counter field within frameOwner: defaults to 'framenum'

It is the user program responsibility to update __frameOwner[frameName]__.
Frame counting will depend on exactly where in the frame cycle the start call and framenum update happen.

As a shortcut __Gldebug.start()__ may be called with an integer (__frames__) or a string (__action__)

Additionally: __Gldebug.checkglerr()__ may be called at any point of a user program
to check for outstanding gl errors and take appropriate action.

By default Gldebug logging and error messages are directed to the console;
these can be overridden by setting `Gldebug.serious`, `Gldebug.showbaderror`, and `Gldebug.log`

## Example calls:
 * `Gldebug.start()`                                        // check all gl calls and log errors, until stop
 * `Gldebug.start(1)`                                       // check all gl calls for 1 frame and log errors
 * `Gldebug.start({gl, action: 'logall', frames: 1})`       // log all gl calls for 1 frame
 * `Gldebug.stop()`                                         // stop Gldebug, remove all gl call wrappers
 * `Gldebug.checkglerr('test')`                             // explicit call to check current gl error status
