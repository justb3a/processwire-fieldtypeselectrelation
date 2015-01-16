# ProcessWire Fieldtype Select Relation

## Overview:

This Fieldtype creates a select list (drop down) in relation to another field (you can restrict the list by setting up certain templates and / or pages).

## Installation:

1. Clone the module and place FieldtypeSelectRelation in your site/modules/ directory. 

```
git clone https://github.com/justonestep/processwire-fieldtypeselectrelation.git your/path/site/modules/FieldtypeSelectRelation
```

2. Login to ProcessWire admin and click Modules. 
3. Click "Check for new modules".
4. Click "install" next to the new FieldtypeSelectRelation module. 

## Usage:

After installation, you'll have a new "Select" fieldtype that will allow you to define the items you'd like in the drop down depending on another field.

* Create a new field and assign type **SelectRelation**
* Click on the **Details** tab, there you find different fields to create your select list
	* **Field:** required, choose another field you created before from which the select list should be populated
	* **Repeater:** optional, if the field you chose is included in a repeater, select the repeater here
	* **Template(s):** optional, restrict your result by setting certain template(s)
	* **Page(s):** optional, restrict your result by setting certain page(s)
	* **Unique Values:** optional, check this field if you want to avoid duplicate values   
(If you enable this, the string value will be saved instead of the ID. You will not be able to reference via ID.)

### Unique Values Disabled
If this field is not enabled the key is the ``pages_id`` and the value is the ``value from the other field``.

```HTML
<select id="Inputfield_chosen_fruit" name="chosen_fruit">
  <option value=""></option>
  <option selected="selected" value="1026">Strawberry</option>
  <option value="1030">Apple</option>
  <option value="1031">Banana</option>
  <option value="1032">Lemon</option>
</select>
```

*Note:*
If you change the value of a field the value changes too. If you deleted a value the value now is empty and multilingualism works like a charm.

Accessing the value:

```PHP
$pages->get((int)$page->chosen_fruit)->title;
$pages->get((int)$page->chosen_color)->color;
```

``chosen_fruit`` and ``chosen_color`` are of the type **SelectRelation**.  
``title`` is the page title and ``color`` a simple input field (TextLanguage for example).

In ``chosen_fruit`` the selected field is ``title``.
And in ``chosen_color`` the selected field is ``color``.

### Unique Values Enabled
If this field is checked the key as well as the value are the ``value from the other field``.

```HTML
<select id="Inputfield_chosen_color" name="chosen_color">
  <option value=""></option>
  <option value="red">red</option>
  <option value="green">green</option>
  <option selected="selected" value="yellow">yellow</option>
</select>
```

If you don't check this box you might have duplicate colors (because there is more than one fruit which has a yellow color for example).  
**BUT** if you change the value of an field, your already chosen value stays the same, because there isn't a relation via ID.

Access such a field like you are used to:

```PHP
$page->chosen_color;
```

**TL;DR:**
I tried to save a comma separated list for duplicate values .It works until you change anything.
For example, if you have two times the color yellow a key of the kind '1034,1036' may get saved.
Now assume you change a basic color (the banana turns brown :D), the selected value is now empty because a key like '1034,1036' doesn't exist anymore.
And in the frontend you may get a wrong output. 