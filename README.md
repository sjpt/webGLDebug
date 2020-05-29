 * Gldebug implements a mechanism for wrapping and monitoring all gl calls.
 * Gldebug.start(opts) applies a wrapper to all gl and gl extension functions.
 * Gldebug.stop() restores the original gl builtin functions.

Gldebug imposes a very significant overhead so should be used sparingly,
but can be very helpful for debugging without the need to make lots of code changes.

The wrapper can check for gl errors before (should be unnecessary) and after every gl call.
The wrapper logs some statistics, and takes optional actions, depending on 'action' and any error found.
 * 'action' is a string that can contain one or more of
 *  logall:         all gl calls are logged
 *  logerr:         gl error calls are logged
 *  breakall:       debugger break on every gl call
 *  breakerr:       debugger break on error gl calls
 *  checkbefore:    checks are carried out before each call (by default they are not)
 *  nocheckafter:   checks are not carried out after each call (by default they are)

Gldebug.start() takes an options object as input, which can contain
 *  gl:         gl context, if not given this may be deduced from window.sk or Gldebug.gl
 *  action:     action string as above: defaults to 'logerr'
 *  frames:     number of frames to debug before automatic stop: defaults to Infinity.
 *  frameOwner: object in which the frame counter lives: defaults to window
 *  frameName:  name of frame counter field within frameOwner: defaults to 'framenum'
 *              It is the user program responsibility to update frameOwner[frameName].
 *              Framecounting will depend on exactly where in the frame cycle the start call and framenum update happen.
 *
As a shortcut Gldebug.start() may be called with an integer (frames) or a string (action)
 *
Example calls:
 * Gldebug.start(1)                                     // check all gl calls and log errors, until stop
 * Gldebug.start(1)                                     // check all gl calls for 1 frame and log errors
 * Gldebug.start({gl, action: 'logall', frames: 1})     // leg all gl calls for 1 frame
 *
##
Additionally: Gldebug.checkglerr() may be called at any point of a user program
      to check for outstanding gl errors and take appropriate action.
