# redux linear undo

This is a tiny extension of the [`redux-undo`](https://github.com/omnidan/redux-undo) package
that adds a `linearizeHistory` config option. Linearizing history allows you to "undo undos",
giving you a completely linear history in which "future" states are never lost (this is
Emacs-style undo) - if you keep pressing the undo button you'll be taken through the entire
history of edits.

# Example

Say we have a simple application that lets us increment, decrement, and print out a number. We
start with `1` and increment it 4 times to get to `5`. Now our past states look like this:

`[1, 2, 3, 4]`

If we then undo 2 times we get back to `3` and our past looks like this:

`[1, 2]`

and our future looks like this:

`[4, 5]`

Dispatching any action except for an undo will trigger a linearization of the history. So if
we now print, our history will first be linearized to look like this:

`[1, 2, 3, 4, 5, 4]`

Now the next undo action will undo the previous undo, taking us back to `4` instead of `2`.

# Usage

The usage is the same as for [`redux-undo`](https://github.com/omnidan/redux-undo#making-your-reducers-undoable).
Just pass the `linearizeHistory` configuration option.

```
// redux-undo higher-order reducer
import undoable from 'redux-undo';

combineReducers({
  counter: undoable(counter, { linearizeHistory: true })
});
```