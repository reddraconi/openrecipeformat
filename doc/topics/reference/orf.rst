Open Recipe Format Reference
============================


oven_fan
--------
Setting to be used with convection ovens. Possible values are "Off", "Low" and
"High". If not specified, it is assumed to be "Off". If specified, all software
should display and print this value. If not specified, it is up to the software
whether or not it is displayed and/or printed, but it should be consistent.

prep_time
---------
Estimated time in H:M taken to prepare the ingredients for the recipe. This 
should include time for marinating, etc.

oven_temp
---------
Starting oven temperature, if the oven is used.

amount
~~~~~~
The number of degrees.

unit
~~~~
Must be "C" for degrees Celsius or "F" for degrees Fahrenheit

oven_time
---------
How long the dish should spend in the oven. This is an overall value, which
refers to the recipe as a whole. If multiple oven times are used, they should
be specified in the recipe.

ingredients
-----------
A list of dicts, defining which food items are to be added to the recipe. These
items should be listed in the order in which they are to be used. Bearing this
in mind, a particular item may be listed multiple times, if it is to be used
multiple times and/or at different quantities in a recipe.

To be clear, it is preferable to list "1 1/2 cups of sugar" and then "1/2 cup
of sugar" (as specified below) than to list "2 cups sugar, divided".

ingredient
~~~~~~~~~~
A dict of items, describing an ingredient, and how much of that ingredient to
use. Multiple ``ingredient_set`` dicts can be present if each has defined a
child ``ingredient_set`` field.

ingredient_set
``````````````
A string used to annotate a group of related ingredients together. This could
be "dry ingredients" and "wet ingredients", "cake" and "frosting", etc.

If this field is not present, and there are more than one ingredient dicts,
then this field is *required*!

amounts
```````
A list of dicts which describe the amounts to use. Normally, the list will only
contain one dict. In cases where multiple yields need to be stored (i.e. 50
cookies vs 100 cookies vs 250 cookies), each yield will have its own dict in
this list, in the same order as the recipe's yield field.

amount
******
The amount of the ``unit`` to use.

unit
****
The unit, relevant to the ``amount``.

processing
``````````
A list of tags which describe the processing of this item. For instance,
"whole", "large dice", "minced", "raw", "steamed", etc.

notes
`````
Any notes specific to this ingredient.

substitutions
`````````````
This field is a list of ingredients, in exactly the same format as a regular
ingredient list item, minus the ``substitutions`` field. For instance, it must
contain ``amounts``, and may also contain ``processing``, ``usda_num``,
``notes``, etc.

usda_num
````````
This corresponds with the index keys in the USDA Standard Reference. It is
generally used for easy lookup of nutritional data. If possible, this should
be used, and USDA data, when available, is preferable to any other nutritional
data source.

notes
-----
This is a field that will appear in several locations. The recipe itself may
have noted, each ingredient may have notes, and each step may have notes.

recipe_name
-----------
The name of this recipe.

recipe_uuid
-----------
This functions somewhat like a network MAC address. It needs to contain an
identifier for the company or the software package, and then a unique identifier
for the recipe itself. This is easiest to handle when a recipe is managed either
by a website (which likely has its own internal primary keys) or a piece of
commercial software that has been registered to a user, using a registration
key.

Ideally, a central source should be set up for companies to register their
unique company identifier, to avoid UUID collisions.

source_book
-----------
If this recipe was originally pulled from a book, then the book information
should go here. Recipe software should make an intelligent effort to include
correct information in the correct fields, rather than just dumping everything
into a generic notes field.

authors
~~~~~~~
This is a list. Refers to the author(s) of this recipe. Can be the same as
``source_authors``, if appropriate. If there was only one author, then they
would be the only item in the list.

title
~~~~~
Title of the book. This is a single value, not a list.

isbn
~~~~
International Standard Book Number, if available.

notes
~~~~~
Any information about the book that does not fit into another field.

X-<field>
~~~~~~~~~
A lot of different information about a book can be stored. Until a field has
been officially accepted into the spec, it should start with a capital X,
followed by a dash.

source_authors
--------------
Does not refer to the person who entered the recipe; only refers to the original
author of the recipe. If this recipe was based on another recipe by another
person, then this field should contain the name of the original author.

source_url
----------
The URL that this recipe was copied from, if applicable. In the case of a
recipe-hosting website, this may refer to the official URL at which the recipe
is hosted.

steps
-----
A list, in order, of steps to be performed on the recipe. Each item in the list
is a dict, as specified below.

step
~~~~
The only item in the dict that is absolutely required.

haccp
~~~~~
A dict, which can contain either a ``control_point`` or a
``critical_control_point``. Should not contain both.

control_point
`````````````
Refers to specific HACCP_ guidelines relevant to this step.

critical_control_point
``````````````````````
Refers to specific HACCP_ guidelines relevant to this step, which are critical
to the safety outcome of this recipe. For instance, "Cook until the food
reaches an internal temperature of 165F."

notes
~~~~~
A list of notes relevant to this step. Often known as "bench notes" to
professionals.

yields
------
Refers to how much food the recipe makes. This is a list, which will normally
contain one dict. In cases where multiple yields need to be stored (i.e. 50
cookies vs 100 cookes vs 250 cookies), each yield will have its own dict in this
list.

amount
~~~~~~
The amount, relevant to the ``unit``.

unit
~~~~
Generally "servings", but up to the user. Can be "packages", "cups", "glasses",
etc.

recommended units
~~~~~~~~~~~~~~~~~
- Serving(s): serving(s)
- Tablespoon(s): T, tbl, or tbsp(s)
- Teaspoon(s): t or tsp(s)
- Gallon(s): gal(s) or gallon(s)
- Quart(s): qt(s) or quart(s)
- Pint(s): pt(s) or pint(s)
- Cup(s): c or cup(s)
- Ounce(s): oz or ounce(s)
- Gram(s): g or gram(s)
- Milliliter(s): ml or milliliter(s)
- Liter(s): L or liter(s)

unrecommended units
~~~~~~~~~~~~~~~~~~~
- Can(s): can(s)
  - Not recommended. Use grams, ounces, cups, or milliliters if possible.
    This can be added under ``notes`` for the specific ingredient if necessary.
    Cans do not hold a standard volume
  - Software should display this unit as "can(s)"
  - Example:

.. code:: yaml
  ingredients:
    - ingredient_set: Bread
    - ingredient: Diced Tomatoes
      usda_num: 00000
      amounts:
        - amount: 36
          unit: oz
      notes: 
        - One 36oz can or Two 18oz cans.
        - Roasted with peppers is highly recommended.

- Package(s): pkg(s) or package(s)
  - Not recommended. Use grams, ounces, or milliliters if possible. Packages can
    be added under "Notes" for the specific ingredient if necessary. Packages
    do not contain a standard volume.
  - Software should display this unit as "package(s)"

- Glass(es): glass(es)
  - Not recommended. Use grams, ounces, cups, or milliliters if possible.
    Glasses do hold a standard volume.
  - Software should display this unit as "glass(es)"

X-<field>
---------
A lot of different information about a recipe can be stored. Until a field has
been officially accepted into the spec, it should start with a capital X,
followed by a dash.

.. _HACCP: https://www.fda.gov/food/guidance-regulation-food-and-dietary-supplements/hazard-analysis-critical-control-point-haccp
