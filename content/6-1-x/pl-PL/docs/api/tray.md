## Klasa: Tray

> Dodaje ikonę i menu kontekstowe do obszaru powiadomień systemu.

Proces: [Main](../glossary.md#main-process)

`Tray` is an [EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter).

```javascript
const { app, Menu, Tray } = require('electron')

let tray = null
app.on('ready', () => {
  tray = new Tray('/path/to/my/icon')
  const contextMenu = Menu.buildFromTemplate([
    { label: 'Item1', type: 'radio' },
    { label: 'Item2', type: 'radio' },
    { label: 'Item3', type: 'radio', checked: true },
    { label: 'Item4', type: 'radio' }
  ])
  tray.setToolTip('This is my application.')
  tray.setContextMenu(contextMenu)
})
```

__Ograniczenia dotyczące platformy:__

* On Linux the app indicator will be used if it is supported, otherwise `GtkStatusIcon` will be used instead.
* On Linux distributions that only have app indicator support, you have to install `libappindicator1` to make the tray icon work.
* App indicator will only be shown when it has a context menu.
* When app indicator is used on Linux, the `click` event is ignored.
* On Linux in order for changes made to individual `MenuItem`s to take effect, you have to call `setContextMenu` again. Na przykład:

```javascript
const { app, Menu, Tray } = require('electron')

let appIcon = null
app.on('ready', () => {
  appIcon = new Tray('/path/to/my/icon')
  const contextMenu = Menu.buildFromTemplate([
    { label: 'Item1', type: 'radio' },
    { label: 'Item2', type: 'radio' }
  ])

  // Make a change to the context menu
  contextMenu.items[1].checked = false

  // Call this again for Linux because we modified the context menu
  appIcon.setContextMenu(contextMenu)
})
```
* W systemie Windows zalecane jest użycie ikony `ICO`, aby uzyskać najlepsze efekty wizualne.

If you want to keep exact same behaviors on all platforms, you should not rely on the `click` event and always attach a context menu to the tray icon.


### `new Tray(image)`

* `image` ([NativeImage](native-image.md) | String)

Tworzy nową ikonę skojarzoną z `image`.

### Zdarzenia instancji

Moduł `Tray` emituje następujące zdarzenia:

#### Zdarzenie: 'click'

* `event` [KeyboardEvent](structures/keyboard-event.md)
* `bounds` [Rectangle](structures/rectangle.md) - The bounds of tray icon.
* `position` [Point](structures/point.md) - The position of the event.

Emitted when the tray icon is clicked.

#### Zdarzenie: 'right-click' _macOS_ _Windows_

* `event` [KeyboardEvent](structures/keyboard-event.md)
* `bounds` [Rectangle](structures/rectangle.md) - The bounds of tray icon.

Emitted when the tray icon is right clicked.

#### Zdarzenie: 'double-click' _macOS_ _Windows_

* `event` [KeyboardEvent](structures/keyboard-event.md)
* `bounds` [Rectangle](structures/rectangle.md) - The bounds of tray icon.

Emitted when the tray icon is double clicked.

#### Zdarzenie: 'balloon-show'_Windows_

Emitted when the tray balloon shows.

#### Zdarzenie: 'balloon-click' _Windows_

Emitted when the tray balloon is clicked.

#### Zdarzenie: 'balloon-closed' _Windows_

Emitted when the tray balloon is closed because of timeout or user manually closes it.

#### Zdarzenie: 'drop' _macOS_

Emitted when any dragged items are dropped on the tray icon.

#### Zdarzenie: 'drop-files' _macOS_

* `event` Event
* `files` String[] - The paths of the dropped files.

Emitted when dragged files are dropped in the tray icon.

#### Zdarzenie: 'drop-text' _macOS_

* `event` Event
* `text` String - the dropped text string.

Emitted when dragged text is dropped in the tray icon.

#### Zdarzenie: 'drag-enter' _macOS_

Emitted when a drag operation enters the tray icon.

#### Zdarzenie: 'drag-leave' _macOS_

Emitted when a drag operation exits the tray icon.

#### Zdarzenie: 'drag-end' _macOS_

Emitted when a drag operation ends on the tray or ends at another location.

#### Zdarzenie: 'mouse-enter' _macOS_

* `event` [KeyboardEvent](structures/keyboard-event.md)
* `position` [Point](structures/point.md) - The position of the event.

Emitted when the mouse enters the tray icon.

#### Zdarzenie: 'mouse-leave' _macOS_

* `event` [KeyboardEvent](structures/keyboard-event.md)
* `position` [Point](structures/point.md) - The position of the event.

Emitted when the mouse exits the tray icon.

#### Zdarzenie: 'mouse-move' _macOS_

* `event` [KeyboardEvent](structures/keyboard-event.md)
* `position` [Point](structures/point.md) - The position of the event.

Emitted when the mouse moves in the tray icon.

### Metody instancji

The `Tray` class has the following methods:

#### `tray.destroy()`

Destroys the tray icon immediately.

#### `tray.setImage(image)`

* `image` ([NativeImage](native-image.md) | String)

Sets the `image` associated with this tray icon.

#### `tray.setPressedImage(image)` _macOS_

* `image` ([NativeImage](native-image.md) | String)

Sets the `image` associated with this tray icon when pressed on macOS.

#### `tray.setToolTip(toolTip)`

* `toolTip` String

Ustawia tekst w dymku ikony w zasobniku systemowym.

#### `tray.setTitle(title)` _macOS_

* `title` String

Sets the title displayed next to the tray icon in the status bar (Support ANSI colors).

#### `tray.getTitle()` _macOS_

* `title` String

Returns `String` - the title displayed next to the tray icon in the status bar

#### `tray.setHighlightMode(mode)` _macOS_

* `mode` String - Highlight mode with one of the following values:
  * `selection` - Highlight the tray icon when it is clicked and also when its context menu is open. This is the default.
  * `always` - Always highlight the tray icon.
  * `never` - Never highlight the tray icon.

Sets when the tray's icon background becomes highlighted (in blue).

**[Przestarzałe](breaking-changes.md#tray)**

**Note:** You can use `highlightMode` with a [`BrowserWindow`](browser-window.md) by toggling between `'never'` and `'always'` modes when the window visibility changes.

```javascript
const { BrowserWindow, Tray } = require('electron')

const win = new BrowserWindow({ width: 800, height: 600 })
const tray = new Tray('/path/to/my/icon')

tray.on('click', () => {
  win.isVisible() ? win.hide() : win.show()
})
win.on('show', () => {
  tray.setHighlightMode('always')
})
win.on('hide', () => {
  tray.setHighlightMode('never')
})
```

#### `tray.setIgnoreDoubleClickEvents(ignore)` _macOS_

* `ignore` Boolean

Sets the option to ignore double click events. Ignoring these events allows you to detect every individual click of the tray icon.

This value is set to false by default.

#### `tray.getIgnoreDoubleClickEvents()` _macOS_

Returns `Boolean` - Whether double click events will be ignored.

#### `tray.displayBalloon(options)` _Windows_

* `options` Object
  * `icon` ([NativeImage](native-image.md) | String) (opcjonalne) -
  * `title` String
  * `content` String

Wyświetla dymek w zasobniku systemowym.

#### `tray.popUpContextMenu([menu, position])` _macOS_ _Windows_

* `menu` Menu (optional)
* `position` [Point](structures/point.md) (optional) - The pop up position.

Pops up the context menu of the tray icon. When `menu` is passed, the `menu` will be shown instead of the tray icon's context menu.

The `position` is only available on Windows, and it is (0, 0) by default.

#### `tray.setContextMenu(menu)`

* `menu` Menu | null

Sets the context menu for this icon.

#### `tray.getBounds()` _macOS_ _Windows_

Zwraca [`Rectangle`](structures/rectangle.md)

The `bounds` of this tray icon as `Object`.

#### `tray.isDestroyed()`

Returns `Boolean` - Whether the tray icon is destroyed.
