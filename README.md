react-burger-menu [![Build Status](https://travis-ci.org/negomi/react-burger-menu.svg?branch=main)](https://travis-ci.org/negomi/react-burger-menu) [![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
=================

An off-canvas sidebar React component with a collection of effects and styles using CSS transitions and SVG path animations.

*Using Redux? Check out [redux-burger-menu](https://github.com/negomi/redux-burger-menu) for easy integration of react-burger-menu into your project.*

## Demo & examples

Live demo: [negomi.github.io/react-burger-menu](https://negomi.github.io/react-burger-menu/)

To build the examples locally, first make sure you're using Node <11.0.0. Then run:

```
npm install
npm start
```

Then open [`localhost:8000`](http://localhost:8000) in a browser.

## Tests

The test suite uses [Mocha](https://mochajs.org/), [Chai](https://chaijs.com/) and [Sinon](https://sinonjs.org/), with [jsdom](https://github.com/tmpvar/jsdom).

To run the tests once, run:

```
npm test
```

To run them with a watcher, run:

```
npm run test:watch
```

## Installation

The easiest way to use react-burger-menu is to install it from npm and include it in your own React build process (using [Browserify](https://browserify.org), [Webpack](https://webpack.github.io/), etc).

You can also use the standalone build by including `dist/react-burger-menu.js` in your page. If you use this, make sure you have already included React, and it is available as a global variable.

Version 3.x uses Hooks, so if you're using React 16.8+:
```
npm install react-burger-menu --save
```

If you're using an earlier version of React:
```
npm install react-burger-menu@^2.9.2 --save
```

If you are using yarn:
```
yarn add react-burger-menu
```

## Props

The Menu component can accept these props:

Prop | Type | Description
--- | :---: | :---:
`isOpen` | `boolean` | Determines whether or not the menu is open
`onStateChange` | `function` | Callback function when there is a state change
`width` | `string`/`number` | Width of the menu (ex: "70%", 300)
`pageWrapId` | `string` | ID of the element wrapping the page content (needed for animations)
`outerContainerId` | `string` | ID of the element conatining everything (needed for animations)

## Usage

Items for the sidebar should be passed as child elements of the component using JSX.

``` javascript
import { slide as Menu } from 'react-burger-menu'

class Example extends React.Component {
  showSettings (event) {
    event.preventDefault();
    .
    .
    .
  }

  render () {
    // NOTE: You also need to provide styles, see https://github.com/negomi/react-burger-menu#styling
    return (
      <Menu>
        <a id="home" className="menu-item" href="/">Home</a>
        <a id="about" className="menu-item" href="/about">About</a>
        <a id="contact" className="menu-item" href="/contact">Contact</a>
        <a onClick={ this.showSettings } className="menu-item--small" href="">Settings</a>
      </Menu>
    );
  }
}
```

### Animations

The example above imported `slide` which renders a menu that slides in on the page when the burger icon is clicked. To use a different animation you can substitute `slide` with any of the following (check out the [demo](https://negomi.github.io/react-burger-menu/) to see the animations in action):

* `slide`
* `stack`
* `elastic`
* `bubble`
* `push`
* `pushRotate`
* `scaleDown`
* `scaleRotate`
* `fallDown`
* `reveal`

### Properties

Some animations require certain other elements to be on your page:

* **Page wrapper** - an element wrapping the rest of the content on your page (except elements with fixed positioning - see [the wiki](https://github.com/negomi/react-burger-menu/wiki/FAQ#i-have-a-fixed-header-but-its-scrolling-with-the-rest-of-the-page-when-i-open-the-menu) for details), placed after the menu component

  ``` javascript
  <Menu pageWrapId={ "page-wrap" } />
  <main id="page-wrap">
    .
    .
    .
  </main>
  ```

* **Outer container** - an element containing everything, including the menu component
  ``` javascript
  <div id="outer-container">
    <Menu pageWrapId={ "page-wrap" } outerContainerId={ "outer-container" } />
    <main id="page-wrap">
      .
      .
      .
    </main>
  </div>
  ```

If you are using an animation that requires either/both of these elements, you need to give the element an ID, and pass that ID to the menu component as the `pageWrapId` and `outerContainerId` props respectively.

Check this table to see which animations require these elements:

Animation | `pageWrapId` | `outerContainerId`
--- | :---: | :---:
`slide` | |
`stack` | |
`elastic` | &#x2713; | &#x2713;
`bubble` | |
`push` | &#x2713; | &#x2713;
`pushRotate` |  &#x2713;  |  &#x2713;
`scaleDown` |  &#x2713;  |  &#x2713;
`scaleRotate` |  &#x2713;  |  &#x2713;
`fallDown` |  &#x2713;  |  &#x2713;
`reveal` |  &#x2713;  |  &#x2713;

#### Position

The menu opens from the left by default. To have it open from the right, use the `right` prop. It's just a boolean so you don't need to specify a value. Then set the position of the button using CSS.

``` javascript
<Menu right />
```

#### Width

You can specify the width of the menu with the `width` prop. The default is `300`.

``` javascript
<Menu width={ 280 } />
<Menu width={ '280px' } />
<Menu width={ '20%' } />
```

#### Open state

You can control whether the sidebar is open or closed with the `isOpen` prop. This is useful if you need to close the menu after a user clicks on an item in it, for example, or if you want to open the menu from some other button in addition to the standard burger icon. The default value is `false`.

``` javascript
// To render the menu open
<Menu isOpen />
<Menu isOpen={ true } />

// To render the menu closed
<Menu isOpen={ false } />
```

You can see a more detailed example of how to use `isOpen` [here](https://github.com/negomi/react-burger-menu/wiki/FAQ#i-want-to-control-the-open-state-programmatically-but-i-dont-understand-how-to-use-the-isopen-prop).

*Note: If you want to render the menu open initially, you will need to set this property in your parent component's `componentDidMount()` function.*

#### Open menu handler

If you keep the menu state yourself it might be convenient to pass a custom function to be used when the user triggers something that should open the menu.

Called when:
* The user clicks on the burger icon

``` javascript
<Menu onOpen={ handleOnOpen } />
```

*Note: The menu will NOT open automatically if you pass this prop, so you must handle it yourself.*

#### Close menu handler

If you keep the menu state yourself it might be convenient to pass a custom function to be used when the user triggers something that should close the menu.

Called when:
* The user clicks on the cross icon
* The user clicks on the overlay
* The user hits the escape key

``` javascript
<Menu onClose={ handleOnClose } />
```

*Note: The menu will NOT close automatically if you pass this prop, so you must handle it yourself.*

#### State change

You can detect whether the sidebar is open or closed by passing a callback function to `onStateChange`. The callback will receive an object containing the new state as its first argument.

``` javascript
var isMenuOpen = function(state) {
  return state.isOpen;
};

<Menu onStateChange={ isMenuOpen } />
```

#### Close on Escape

By default, the menu will close when the Escape key is pressed. To disable this behavior, you can pass the `disableCloseOnEsc` prop. This is useful in cases where you want the menu to be open all the time, for example if you're implementing a responsive menu that behaves differently depending on the browser width.

``` javascript
<Menu disableCloseOnEsc />
```

#### Custom `keydown` handler

For more control over global keypress functionality, you can override the handler that this component sets for `window.addEventListener('keydown', handler)`, and pass a custom function. This could be useful if you are using multiple instances of this component, for example, and want to implement functionality to ensure that a single press of the Escape key closes them all.

``` javascript
const closeAllMenusOnEsc = (e) => {
  e = e || window.event;

  if (e.key === 'Escape' || e.keyCode === 27) {
    this.setState({areMenusOpen: false});
  }
};

<MenuOne customOnKeyDown={closeAllMenusOnEsc} isOpen={areMenusOpen} />
<MenuTwo customOnKeyDown={closeAllMenusOnEsc} isOpen={areMenusOpen} />
```
*Note: Using this prop will disable all the default 'close on Escape' functionality, so you will need to handle this (including determining which key was pressed) yourself.*

#### Overlay

You can turn off the default overlay with `noOverlay`.

``` javascript
<Menu noOverlay />
```

You can disable the overlay click event (i.e. prevent overlay clicks from closing the menu) with `disableOverlayClick`. This can either be a boolean, or a function that returns a boolean.

``` javascript
<Menu disableOverlayClick />
<Menu disableOverlayClick={() => shouldDisableOverlayClick()} />
```

#### Transitions

You can disable all transitions/animations by passing `noTransition`.

``` javascript
<Menu noTransition />
```

This is useful if you want the menu to remain open across re-mounts, for example during SPA route changes.

#### Custom icons

You can replace the default bars that make up the burger and cross icons with custom `ReactElement`s. Pass them as the `customBurgerIcon` and `customCrossIcon` props respectively.

```javascript
<Menu customBurgerIcon={ <img src="img/icon.svg" /> } />
<Menu customCrossIcon={ <img src="img/cross.svg" /> } />
```

You should adjust their size using the `.bm-burger-button` and `.bm-cross-button` classes, but the element itself will have the class `.bm-icon` or `.bm-cross` if you need to access it directly.

You can also disable the icon elements so they won't be included at all, by passing `false` to these props.

```javascript
<Menu customBurgerIcon={ false } />
<Menu customCrossIcon={ false } />
```

This can be useful if you want exclusive external control of the menu, using the `isOpen` prop.

#### Custom ID and/or classNames

There are optional `id` and `className` props, which will simply add an ID or custom className to the rendered menu's outermost element. This is not required for any functionality, but could be useful for things like styling with CSS modules.

``` javascript
<Menu id={ "sidebar" } className={ "my-menu" } />
```

You can also pass custom classNames to the other elements:

``` javascript
<Menu burgerButtonClassName={ "my-class" } />
<Menu burgerBarClassName={ "my-class" } />
<Menu crossButtonClassName={ "my-class" } />
<Menu crossClassName={ "my-class" } />
<Menu menuClassName={ "my-class" } />
<Menu morphShapeClassName={ "my-class" } />
<Menu itemListClassName={ "my-class" } />
<Menu overlayClassName={ "my-class" } />
```

And to the `html` and `body` elements (applied when the menu is open):
```javascript
<Menu htmlClassName={ "my-class" } />
<Menu bodyClassName={ "my-class" } />
```

*Note: Passing these props will prevent the menu from applying styles to the `html` or `body` elements automatically. See [here](https://github.com/negomi/react-burger-menu/wiki/FAQ#why-is-overflow-x-hidden-being-set-on-the-html-and-body-elements) for more explanation.*

#### Focusing the first menu item

By default, the menu will set focus on the first item when opened. This is to help with keyboard navigation. If you don't want this functionality, you can pass the `disableAutoFocus` prop.

``` javascript
<Menu disableAutoFocus />
```

#### Custom item list element

The menu's children are all wrapped in a `nav` element by default, as navigation is likely the most common use case for this component. However, it's a general purpose sidebar, so you can change this to a `div` if you're not using it for navigation:

``` javascript
<Menu itemListElement="div" />
```

### Styling

All the animations are handled internally by the component. However, the visual styles (colors, fonts etc.) are not, and need to be supplied, either with CSS or with a JavaScript object passed as the `styles` prop.

#### CSS

The component has the following helper classes:

``` css
/* Position and sizing of burger button */
.bm-burger-button {
  position: fixed;
  width: 36px;
  height: 30px;
  left: 36px;
  top: 36px;
}

/* Color/shape of burger icon bars */
.bm-burger-bars {
  background: #373a47;
}

/* Color/shape of burger icon bars on hover*/
.bm-burger-bars-hover {
  background: #a90000;
}

/* Position and sizing of clickable cross button */
.bm-cross-button {
  height: 24px;
  width: 24px;
}

/* Color/shape of close button cross */
.bm-cross {
  background: #bdc3c7;
}

/*
Sidebar wrapper styles
Note: Beware of modifying this element as it can break the animations - you should not need to touch it in most cases
*/
.bm-menu-wrap {
  position: fixed;
  height: 100%;
}

/* General sidebar styles */
.bm-menu {
  background: #373a47;
  padding: 2.5em 1.5em 0;
  font-size: 1.15em;
}

/* Morph shape necessary with bubble or elastic */
.bm-morph-shape {
  fill: #373a47;
}

/* Wrapper for item list */
.bm-item-list {
  color: #b8b7ad;
  padding: 0.8em;
}

/* Individual item */
.bm-item {
  display: inline-block;
}

/* Styling of overlay */
.bm-overlay {
  background: rgba(0, 0, 0, 0.3);
}
```

#### JavaScript

The same styles can be written as a JavaScript object like this:

```javascript
var styles = {
  bmBurgerButton: {
    position: 'fixed',
    width: '36px',
    height: '30px',
    left: '36px',
    top: '36px'
  },
  bmBurgerBars: {
    background: '#373a47'
  },
  bmBurgerBarsHover: {
    background: '#a90000'
  },
  bmCrossButton: {
    height: '24px',
    width: '24px'
  },
  bmCross: {
    background: '#bdc3c7'
  },
  bmMenuWrap: {
    position: 'fixed',
    height: '100%'
  },
  bmMenu: {
    background: '#373a47',
    padding: '2.5em 1.5em 0',
    fontSize: '1.15em'
  },
  bmMorphShape: {
    fill: '#373a47'
  },
  bmItemList: {
    color: '#b8b7ad',
    padding: '0.8em'
  },
  bmItem: {
    display: 'inline-block'
  },
  bmOverlay: {
    background: 'rgba(0, 0, 0, 0.3)'
  }
}

<Menu styles={ styles } />
```

## Contribution Guidelines
We take contributions! Here is how you get started:
#### 1. Fork the repository: Click the fork button at the top-right of the project repository.

#### 2. Clone your fork:
```
git clone https://github.com/your-username/react-burger-menu.git
```

#### 3. Install dependencies:
```
npm install
```

#### 4. Create a new branch:
```
git checkout -b example-branch-name
```

#### 5. Make changes and commit them:
```
git commit -m "Leave a detailed description here!"
```

#### 6. Push your branch to GitHub:
```
git push origin example-branch-name
```

#### 7. Submit a pull request on the main repository!

### Browser support

Because this project uses CSS3 features, it's only meant for modern browsers. Some browsers currently fail to apply some of the animations correctly.

Chrome and Firefox have full support, but Safari and IE have strange behavior for some of the menus.

### Help

Check the FAQ (https://github.com/negomi/react-burger-menu/wiki/FAQ) to see if your question has been answered already, or [open a new issue](https://github.com/negomi/react-burger-menu/issues).

### License

MIT
