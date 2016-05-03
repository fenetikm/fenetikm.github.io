# Drupal <img src="images/d8-logo.png" alt="Drupal 8 logo" width="100" style="background:none; border:none; margin: 0; box-shadow: none">

## Controlled Fields
aka Conditional Fields but that name and similar names are taken

Michael Welford

<michael.welford@adelaide.edu.au>

---

## Controlled Fields module [WIP]

The Challenge: allow non-technical administrators to 
*easily* set one field to become visible when the value
of another field is something specified.

Limitations: Only required for form display (not view display). 
Only needs to work for select and radio widgets.

---

<!-- .slide: data-background="images/demo.jpg" -->
.

---

## Problem: Required Fields

Q. How to solve? No really, how would you solve this?

- Why not just mangle the form using hooks, preprocess or even javascript? 
- Why not get super crazy and replace the widget with a non-required version by re-rendering it into the form array in an alter?

(hint: these don't work)

---

## Problem: Required Fields
### Current Solution

- To work with Entity Validation we must jump through some hoops:
- On the entity form display ui, check for controlled fields, and save the value of required for all into third party settings, remove the required setting from FieldConfid
- FieldConfig is an entity (yeah!)
  - hook_entity_prepare_form to set required on the form depending on tps
  - hook_entity_presave to setRequired(FALSE) and save into third party settings
- Custom recursive validation, checks to see which fields are showing, checks required setting if applicable
- Add in required class to fields to show what is required 
- (@todo) Add in JS to add the required attribute to input fields

---

## Finer points

- hook_field_widget_settings_summary_alter() to show what is controlled on the entity form_display page
- hook_field_widget_third_party_settings_form() to add in your own custom settings to select the controlling field
- Display Machine Name module to show the machine name of a field next to its label - needed when you have more than one field with the same label

---

## Questions?

---

## Thank you!

Michael Welford

<michael.welford@adelaide.edu.au>
