# Block Types

## General Block Properties

```javascript
({
	title: "String",
	displayTitle: "String"
})
```

All blocks have the properties defined here.

Property|Type|Description
---|---|---
``title`` | String | The name of the block as you'll use it in the code.
``displayTitle`` | String | The name of the block as presented to the end user. If unspecified, uses the value of ``title``.

In the value presented to the end user for the block title, having a ``/`` (forward slash) in the
name causes the system to generate a tree structure to the block instead of presenting it as a flat
list.

## Text

```javascript
new Blocks.Text({
	title: "Text Block",
	multiLine: true
});
```

This creates a text block.

Property|Type|Description
---|---|---
``multiLine`` | Boolean | Whether this field has multiple lines for user input or only one. 

Making a text field multi-line for the user does not automatically make the content present as multiple lines when rendering; if that is necessary, 
you'll have to replace ``\n`` with ``<br>`` or set styles to enforce that (such as ``white-space: pre-wrap;`` in the element's styles).
						  
## RawText

```javascript
new Blocks.RawText({
	title: "Text Block",
	multiLine: true
});
```

This creates a text block. The contents of this block will not automatically have HTML control characters escaped when it's value is called.

Property|Type|Description
---|---|---
``multiLine`` | Boolean | Whether this field has multiple lines for user input or only one. 

## HTML

```javascript
new Blocks.HTML({
	title: 'HTML block',
	asEmbedCode: false
});
```

This creates an HTML input field.

Property | Type | Description
--- | --- | ---
``asEmbedCode`` | Boolean | Whether this field contains HTML to be edited via a WYSIWYG editor or raw HTML for presentation to the end user.

The difference between ``new Blocks.RawText({ multiLine: true })`` and ``new Blocks.HTML({ asEmbedCode: true })`` is minimal, but intended
to state that the former is arbitrary text which may be used programatically, where the latter is still HTML to be sent to the user of the
website containing this block.

## Image

```javascript
new Blocks.Image({
	title: 'Image Block',
	useS3: false
});
```

This creates an image block. The system will not allow a non-image file to be selected for this field.

Property | Type | Description
--- | --- | ---
``useS3`` | Boolean | Use an Amazon S3 bucket (configured elsewhere) for storing files uploaded into this block.

## Document

```javascript
new Blocks.Document({
	title: 'Document Block',
	useS3: false
});
```

This creates a reference to an arbitrary document. This may be an image but is not restricted to being only
an image.

Property | Type | Description
--- | --- | ---
``useS3`` | Boolean | Use an Amazon S3 bucket (configured elsewhere) for storing files uploaded into this block.

## AppGroup

```javascript
new Blocks.AppGroup({
	title: 'App Group Block',
	app: 'APP_ID',
	group: 'GROUP_ID',
	create: true,
	mode: 'list'
});
```

This allows you to specify a group of records.

Property | Type | Description
--- | --- | ---
``app`` | String | The app ID holding the group to select. (This is the value of the ``APP_ID`` const in the app definition under ``/apps/``.)
``group`` | String or Object | The group to be selected. See below for more details.
``create`` | Boolean | Whether to create a group if the desired group does not exist.
``fromURL`` | Boolean | If the selected group is actually chosen as a result of being linked through the page URL instead of being coded by the developer.
``userSelect`` | Boolean | If the selected group is directly selected by the site administrator.
``mode`` | String | The way the group should be presented to the site administrator. One of ``tree`` (full group tree rooted at the selected group), ``list`` (list of records from the selected group) or ``object`` (the group object itself).
``customSource`` | Function | A method which returns a Group object.

To determine the group presented:

* Check ``customSource``. If it exists, the method is called. If it returns a Group object, the group is used.
* Check ``fromURL``. If true, and the page we're looking at has a reference to a group in the specified app, that group is used.
* Check ``userSelect``. If true, we use the group specified directly by the administrator.
* Check ``group``. If ``group`` is not null:
  * If ``group`` is an object with a property ``stub``:
    Look up a group with the value in it's ``stub`` field matching the value in the ``group.stub`` property. Use it if found.
  * If ``group`` is a string:
    Look up a group with the ID specified by the ``group`` property. Use it if found.
* Check ``create``; if true:
  * Create a new group, using the block title and group identifier specified in the ``group`` property and return it.

If ``userSelect`` is true, ``mode`` is ignored (when the end user can directly select the group, the editor presents the group selection instead of any views controlled
by ``mode``.)

## AppRecord

```javascript
new Blocks.AppRecord({
	title: 'Record Block',
	app: 'APP_ID',
	fromURL: true
});
```

This allows you to specify a record to display.

Property | Type | Description
--- | --- | ---
``app`` | String | The app ID holding the reciord to select. (This is the value of the ``APP_ID`` const in the app definition under ``/apps/``.)
``fromURL`` | Boolean | Whether the record is specified by the page URL or not.
``userSelect`` | Boolean | Whether the record is specified by the user or not
``record`` | String | The record ID to be displayed.

Record lookup:
* If ``fromURL`` is true, check for a record from this app in the URL; if found, return it.
* If ``userSelect`` is true, check for an explicit selection made by the administrator and return it if found.
* Otherwise, if ``record`` is specified, return the record with that ID.

Specifying ``userSelect`` will change the view from being that of the record being presented to a record selector in the editor.

# Page API

## blocks():Object

```javascript
exports.Pages[PAGE_ID] = class extends ParentLayout {
	blocks() {
		const Blocks = exports.Blocks;
		return {
			title: "Object Title",
			content: [
				new Blocks.Text({ title: 'Page title' })
			]
		}
	}
}
```

This is your mechanism for telling the system what blocks appear on the page. Define this method
in your pages returning an object as described here.

## storeBlocks():void

```javascript
page.storeBlocks();
```

This method creates the necessary structures in the Content Blocks app for the blocks defined here. In general, it is
automatically invoked by the system as necessary.

## get pageId():String

```javascript
page.pageId
```

This property holds the ID of the current page or layout. In general, it will be in the form of:
* ``BasePage``
* ``Layouts.<Layout ID>``
* ``Pages.<Page ID>``

This is both an instance and static property (the instance property is there for ease of access; it simply
accesses ``this.constructor.pageId``.)

## get parentPageId():String

```javascript
page.parentPageId
```

This property holds the ID of the parent layout. The values are the same as for ``pageId``.

## block(name:String):Block

```javascript
exports.Pages[PAGE_ID] = class extends ParentLayout {
	blocks() {
		const Blocks = exports.Blocks;
		return {
			title: "Object Title",
			content: [
				new Blocks.Text({ title: 'Page title' })
			]
		}
	}
	
	renderSomeContent() {
		return `
			<div>
				<p>${this.block("Page title").resolve()}</p>
			</div>
		`;
	}
}
```

From within a page object, ``block(name)`` retrieves the instance of the content block from the page.

## block(pageId:String, name:String):Block

```javascript
exports.Layouts[Layout_ID] = class extends ParentLayout {
	blocks() {
		const Blocks = exports.Blocks;
		return {
			title: "Layout Title",
			content: [
				new Blocks.Text({ title: 'Layout text' })
			]
		}
	}
	
	renderSomeContent() {
		return `
			<div>
				<p>${this.block(`Layouts.${LAYOUT_ID}`, "Layout text").resolve()}</p>
			</div>
		`;
	}
}
```

This retrieves the named block for the specified layout. The first argument will be typically
something of the form: ``"Layouts." + LAYOUT_ID`` or similar, since the code cannot determine
exactly where in scope the block() method is being called from.

## blockFromPage(pageId:String, blockName:String):Block

```javascript
this.blockFromPage('Home', 'Some List')
```

Retrieve the named block from the page with the specified ID.

## blockFromLayout(layoutId:String, blockName:String):Block

```javascript
this.blockFromLayout("MainLayout", "Some List")
```

Retrieve the named block from the layout with the specified ID.

## getSpecificBlock(pageClass:Page, blockName:String):Block

```javascript
this.getSpecificBlock(exports.Pages.Home, "Some List")
```

This is used behind the scenes by ``blockFromPage`` and ``blockFromLayout``.


# Block API

## get blockType():String
```javascript
block.blockType
```

Retrieve the type of the block. This is the name of one of the block type constructors above.

## get blockId():String
```javascript
block.blockId
```

Retrieve the ID of the block as used in the Content Blocks app.

## get defaultContentObject():Object
```javascript
block.defaultContentObject
```

Retrieve the data used by the blocks API for a default version of this boject.

## save():void
```javascript
block.save()
```

Save this block into the Content Blocks app. This creates the initial instance of the block and updates it's title if needed.

In general, the system will do this for you.

## get block():Record
```javascript
block.block
```

Retrieve the underlying block record. This might be null if the content block object itself has no need of a backing store.

## preprocess(content:String):String
```javascript
block.preprocess(`
	My text
	
	It has multiple lines with some prefix indentation.
`);
```

This method removes the following from a string passed:
* Leading whitespace from all lines in the string.
* An empty line at the beginning of the string.
* An empty line at the end of the string.

## resolve(defaultContent:Any):Any

```javascript
block.resolve(`
	My Text
`);
```

This determines the content that should appear in the block and returns it.

The acceptable values for defaultContent and the return value vary based on the block type:
* Text: The argument is the text to be used, without HTML encoding. The result is always HTML encoded text.
* RawText, HTML: The argument and the result are a string containing text.
* Image, Document: The argument is the path to a file to be used by default. The result is a fully-specified URL to the file.
* AppGroup, AppRecord: The argument is unused; the result is a Group or Record respectively.

## process(callback:(content:Any):Any, defaultContent:Any):Any

```javascript
block.process(value => value.split("\n").join("<br>"), `
	This is some text
	
	with multiple lines
`)
```

This works the same way as ``block.resolve()`` but instead of returning the content, passes it to
the provided callback function and returns it's value instead.

## toJSON():Object

```javascript
block.toJSON()
```

Retrieves the user-visible properties of the block.

## get Image.S3():Boolean, get Document.S3():Boolean

```javascript
block.S3
```

If the block is an Image or Document and it's contents are to be stored in Amazon S3, this is true.

## get noSave()

```javascript
block.noSave
```

If this property is true, the block has no backing store and will not appear in the Content Blocks app.

