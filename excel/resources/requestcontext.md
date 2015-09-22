# RequestContext

The RequestContext object facilitates requests to the Excel application. Since the Office add-in and the Excel application run in two different processes, request context is required to get access to Excel and related objects such as worksheets, tables, etc. from the add-in. 

## Properties
None

## Methods

| Method         | Return Type    |Description|Notes |
|:---------------|:--------|:----------|:-----|
|[load(object: object, option: object)](#loadobject-object-option-object)  |void     |Fills the proxy object created in JavaScript layer with property and options specified in the parameter.||

## API Specification

### load(object: object, option: object)
Fills the proxy object created in JavaScript layer with property and options specified in the parameter.

#### Syntax
```js
requestContextObject.load(object, loadOption);
```

#### Parameters
| Parameter       | Type    |Description|
|:----------------|:--------|:----------|
|object|object|Optional. Specify the name of the object to be loaded.|
|option|[loadOption](loadoption.md)|Optional. Specify the load options such as select, expand, skip and top. Se Load Option object for details.|

#### Returns
void

##### Examples

The following example shows how to read and copy the values from Range A1:A2 to B1:B2.

```js
Excel.run(function (ctx) { 
	var range = ctx.workbook.worksheets.getActiveWorksheet().getRange("A1:A2");
	ctx.load(range, {"select": "address, values", "expand" : "range/format"});
	return ctx.sync().then(function() {
		var myvalues=range.values;
		ctx.workbook.worksheets.getActiveWorksheet().getRange("B1:B2").values = myvalues;
	});
}).then(function() {
		console.log(range.address);
		console.log(range.values);
		console.log(range.format.wrapText);
}).catch(function(error) {
		console.log("Error: " + error);
		if (error instanceof OfficeExtension.Error) {
			console.log("Debug info: " + JSON.stringify(error.debugInfo));
		}
})
```