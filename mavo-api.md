# Mavo API

## Mavo.Permissions

Permissions such as edit, save, add, delete etc.

_Defined in `permissions.js`_

### Members

#### `set(values)`

Set multiple permissions at once

**ARGUMENTS (1) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| values | `Object` | Name-value pairs |

---

#### `on()`

Set a bunch of permissions to true. Chainable.

---

#### `off()`

Set a bunch of permissions to false. Chainable.

---

#### `can(actions, callback, [cannot])`

Fired once at least one of the actions passed can be performed. Kind of like a Promise that can be resolved multiple times.

**ARGUMENTS (3) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| actions | `String` \| `Array<String>` | |
| callback | `Function` | |
| cannot `OPTIONAL` | `Function` | |

---

#### `cannot(actions, callback)`

Fired once **none** of the actions can be performed

**ARGUMENTS (2) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| actions | `String` \| `Array<String>` | |
| callback | `Function` | |

---

#### `observe(actions, value, callback)`

Schedule a callback for when a set of permissions changes value

**ARGUMENTS (3) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| actions | `String` \| `Array<String>` | |
| value | `Boolean` | |
| callback | `Function` | |

---

#### `is(actions, able)`

Compare a set of permissions with true or false. If comparing with true, we want at least one to be true, i.e. OR. If comparing with false, we want ALL to be false, i.e. NOR.

**ARGUMENTS (2) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| actions | `String` \| `Array<String>` | |
| able | `Boolean` | |

---

#### `onchange(callback)`

Monitor all changes on all permissions.

**ARGUMENTS (1) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| callback | `Function` | Called on any permission change. Its context will be this `Mavo.Permissions` object and its parameter a `{action, value}` object. |

---

## Mavo.Backend

Base class for backends to inherit from.

_Defined in `backend.js`_

### Members

#### `new Mavo.Backend(url, options)`

**CONSTRUCTOR**

**ARGUMENTS (2)**
| Name | Type | Description |
| :--- | :--- | :--- |
| url | `String` | The raw value of the mv-storage/mv-source etc attribute. |
| options | `Object` | Options object. Current options are **mavo** and **format**. |

---

#### `mavo`

**PROPERTY**

The Mavo instance that this node belongs to. Same as the mavo argument that was passed to its constructor.

---

## Mavo.Plugins

Static class with helpers for loading or creating plugins.

_Defined in `plugins.js`_

### Members

#### `url`

**STATIC PROPERTY** `String`

Domain from which Plugins are loaded. Default value is "https://plugins.mavo.io".

---

#### `loaded`

**STATIC PROPERTY** `Object`

Plugins currently loaded.

---

#### `plugins`

**STATIC PROPERTY** `Set`

ids of plugins loaded or loading via the mv-plugins attribute.

---

#### `load()`

**STATIC FUNCTION**

Load all plugins called via the mv-plugins attribute. Mostly used internally.

---

#### `register(id, [options])`

**STATIC FUNCTION**

Register a new plugin.

**ARGUMENTS (2) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| id | `String` | Plugin id that will be used to call it via mv-plugins. |
| options `OPTIONAL` | `Object` | Options object. The supported options are: <ul><li>**ready** (Promise):</li><li>**dependencies** (Array<Promise>):</li><li>**init** (Function): Function to execute after all dependencies have loaded.</li><li>**hooks** (Object):</li><li>**extend** (Object): Want to add properties and methods to existing Mavo classes (or override existing methods)? Use this option! Keys are Mavo class names without `Mavo.` (e.g. `"Primitive"`, `"Collection"` etc) and values are objects for the Mavo class.</li></ul> |

---

## Mavo.UI.Bar

Class for the Mavo toolbar.

_Defined in `ui.bar.js`_

### Members

#### `mavo`

**PROPERTY**

The Mavo instance that this node belongs to. Same as the mavo argument that was passed to its constructor.

---

## Mavo.UI.Itembar

Class for the item bar that allows deleting, rearranging, cloning collection items.

_Defined in `ui.itembar.js`_

### Members

#### `mavo`

**PROPERTY**

The Mavo instance that this node belongs to. Same as the mavo argument that was passed to its constructor.

---

## Mavo.UI.Message

Class for messages displayed to the user, e.g. errors, notices etc.

_Defined in `ui.message.js`_

### Members

#### `new Mavo.UI.Message(mavo, message, [options])`

**CONSTRUCTOR**

**ARGUMENTS (3)**
| Name | Type | Description |
| :--- | :--- | :--- |
| mavo | `Mavo` | Mavo instance this message belongs to |
| message | `String` \| `Object` | HTML of the message as a string or HTML element reference or anything that Bliss’ $.contents() accepts. |
| options `OPTIONAL` | `Object` | Options to customize the behavior of the message. <ul><li>**type**: One of the predefined message styles. Possible values: `warning`, `error`. You may find the `.mv-message::before` selector useful here to change/remove/style the text added by Mavo _before_ the message.</li><li>**classes**: Class(es) to add to the message (space-separated). You may find `mv-inline` useful here.</li><li>**dismiss**: If the message is dismissable, what should dismiss it? Options: `"button"`: x close button, `"timeout"` (after 5s or whatever specified), `"submit"` (submit event — useful if the message contains a form)</li></ul> |

---

#### `mavo`

**PROPERTY**

The Mavo instance that this node belongs to. Same as the mavo argument that was passed to its constructor.

---

## Mavo.UI.Popup

Class for the little tooltip-like popups for editing attributes.

_Defined in `ui.popup.js`_

### Members

#### `mavo`

**PROPERTY**

The Mavo instance that this node belongs to. Same as the mavo argument that was passed to its constructor.

---

## Mavo.Observer

Helper class to make Mutation observers more pleasant to deal with.

_Defined in `util.js`_

### Members

#### `new Mavo.Observer(element, attribute, callback, o)`

**CONSTRUCTOR**

**ARGUMENTS (4)**
| Name | Type | Description |
| :--- | :--- | :--- |
| element | `Element` | The element to observe |
| attribute | `String` | The attribute to observe (null if none) |
| callback | `Function` | Callback to execute |
| o | `Object` | Configuration object with any extra options to pass to `MutationObserver#observe()` |

---

#### `observer`

**PROPERTY** `MutationObserver`

---

#### `element`

**PROPERTY** `Element`

---

#### `attribute`

**PROPERTY** `String`

---

#### `callback`

**PROPERTY** `Function`

---

#### `options`

**PROPERTY** `Object`

---

#### `run()`

**FUNCTION**

---

#### `sneak(callback)`

**FUNCTION**

Sneak some code past the observer, whether it’s active or not.

**ARGUMENTS (1) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| callback | `Function` | Code to run |
| **Returns** | | Return value of the callback |

---

#### `destroy()`

**FUNCTION**

Stop & destroy this observer.

---

#### `sneak(observer)`

**STATIC FUNCTION**

Same as the instance method, but can be used whether the observer exists or not. If the observer does not exist, the callback is still executed.

**ARGUMENTS (1) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| observer | `Mavo.Observer` | The observer to call sneak() on |
| **Returns** | | Return value of the callback |

---

## Mavo.Data

Data object attached to each node, responsible for keeping track of "live data", that is, data used for evaluating expressions and such. `Mavo.Data` also includes a lot of logic for name resolution during evaluation of expressions. This is distinct from the data for storing in the backend, which is simpler and is taken care of by `Mavo.Node.getdata()`.

_Defined in `data.js`_

### Members

#### `data`

**PROPERTY**

The underlying data value, e.g. an Array if this `Mavo.Data` is attached to a Collection, usually a string or other primitive value if this `Mavo.Data` is attached to a Primitive. When `data` is a JavaScript object or array, it has some properties keyed by Symbols from Mavo:

- Under `Mavo.route`, the data has an object keyed by property names.
- a `Set` of property names of direct children of the current node with that name, or
- the boolean literal `true`, if all direct children should be looked into, which happens if `data` represents a Collection.
- Under `Mavo.property`, the data has the property name or numerical index for the node.
- Under `Mavo.parent`, the data has the data value from the parent node.

---

#### `parent`

**PROPERTY**

The `Mavo.Data` attached to the parent node.

---

#### `stub`

**STATIC PROPERTY**

A global object that looks up any property accesses in Mavo’s global scope of utility functions and special values.

---

#### `update()`

**FUNCTION**

Update this data object based on children nodes’ data (if the node is a Collection or ImplicitCollection) or the node’s value in the DOM (if the node is a Primitive). Does nothing in other cases. Propagates updates to parent if any changes were made.

---

#### `resolve(property)`

**FUNCTION**

Resolves the value associated with a property name. If the property name directly exists on `data` then it is looked up and returned from there. Otherwise it is looked up among Mavo globals and other fallbacks in name resolution. If JavaScript object data is looked up from a route or a node, it will be returned as a JavaScript proxy on which property lookups will also call `resolve`.

**ARGUMENTS (1) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| property | `String` | Property name to resolve. |

---

#### `findUp(property, data)`

**STATIC FUNCTION**

Looks for property in data’s descendants (including itself), then in the descendants of each ancestor of `data` in turn, until the property is found.

**ARGUMENTS (2) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| property | `String` | Property name to look for. |
| data | `String` | Data object to look in. |

---

#### `find(property, data, [options])`

**STATIC FUNCTION**

Looks for property in data’s descendants (including itself).

**ARGUMENTS (3) & RETURN VALUE**
| Name | Type | Description |
| :--- | :--- | :--- |
| property | `String` | Property name to look for. |
| data | `Object` | Data object to look in. |
| options `OPTIONAL` | `Object` | Object of options. If the key `exclude` is provided, a direct child data object that is identical to `options.exclude` is skipped. |
