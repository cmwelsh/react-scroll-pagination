ScrollPagination
================

[React](http://reactjs.org/) component for handing infinite scroll without infinitely growing the DOM.

## Usage

```javascript
var Page = ScrollPagination.Page;
function handlePageEvent(pageId, event) {
	this.refs.scrollPagination.handlePageEvent(pageId, event);
}
```
```
/** @jsx React.DOM */
<ScrollPagination
	ref="scrollPagination"
	hasPrevPage={ Boolean() }
	hasNextPage={ Boolean() }
	loadPrevPage={ function(){} }
	loadNextPage={ function(){} }
	unloadPage={ function (pageId) {} }>

	<Page id="page-1" onPageEvent={handlePageEvent.bind(this, "page-1")}>
		<h1>Render first page of content here</h1>
	</Page>

	<Page id="page-2" onPageEvent={handlePageEvent.bind(this, "page-2")}>
		<h1>Render second page of content here</h1>
	</Page>
</ScrollPagination>
```

```javascript
function handlePageEvent(pageId, event) {
	this.refs.scrollPagination.handlePageEvent(pageId, event);
}

ScrollPagination({
	ref: "scrollPagination",
	hasPrevPage: Boolean(),
	hasNextPage: Boolean(),
	loadPrevPage: function(){},
	loadNextPage: function(){},
},
	ScrollPagination.Page({
		id: "page-1",
		onPageEvent: handlePageEvent.bind(this, "page-1") },
			React.DOM.h1(null, "Render first page of content here")),
	ScrollPagination.Page({
		id: "page-2",
		onPageEvent: handlePageEvent.bind(this, "page-2") },
			React.DOM.h1(null, "Render second page of content here")))
```

**ScrollPagination:**

- `hasPrevPage` must be a boolean indicating if `loadPrevPage` should be called.

- `hasNextPage` must be a boolean indicating if `loadNextPage` should be called.

- `loadPrevPage` must be a function that causes a re-render with a single page prepended.

- `loadNextPage` must be a function that causes a re-render with a single page appended.

- `unloadPage` must be a function that causes a re-render with the specified page id removed.

**ScrollPagination.Page:**

- `id` must be the id of the page (used for `unloadPage`).

- `onPageEvent` must be a function that calls the `ScrollPagination` instance with `pageId, event` (where `event` is the only argument `onPageEvent` is called with).

## What it does

The scroll container will be determined when the component is first mounted (any parent element with `overflow:auto` or `overflow:scroll`, defaults to `window`).

`loadPrevPage` and `loadNextPage` are called when there is one thrid or less of the current page height out of view above and below respectively.

`unloadPage` is called before `loadPrevPage` or `loadNextPage` if there is a page at the opposite end of the visible area entirely out of view.

Each page calculates it's own height and reports it to the `ScrollPagination` instance via `onPageEvent`. The `ScrollPagination` instance keeps track of what pages are added or removed from the top and adjusts the scroll position.

## Why

Infinite scroll implementations where DOM nodes are continually added without cleaning up cause the DOM to get __very__ slow the farther you scroll down (especially when you have any kind of dynamic content). Removing what you don't need from the DOM and replacing it with padding allows scrolling through pages without slowing down the DOM.

## How you can help

File an [issue](https://github.com/cupcake/react-scroll-pagination/issues) if you find anything isn't working as expected.

Pull requests are always welcome, but you should open an issue before working on a new feature (both to ensure it's within the scope of this project and that it's not already being worked on).

