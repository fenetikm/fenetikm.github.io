# Drupal <img src="images/d8-logo.png" alt="Drupal 8 logo" width="100" style="background:none; border:none; margin: 0; box-shadow: none">

## Scholarships Post Mortem
### or How I learned to Stop Worrying and Love the Form

Michael Welford

<michael.welford@adelaide.edu.au>

---

## Scholarship Project tldr;

"Let's take the Scholarship application process online"

Approx. 100ish Scholarships, 40 or so with online forms.

Scholarship applications in Peoplesoft

Drupal 8 plz

---

## Overview of the talk

Why? Share the Drupal knowledge.

- Challenges
- Non-challenges
- Some Assembly Required
- Dogfooding
- Reusable modules

(No Code!)

---

<!-- .slide: data-background="images/rocky.jpg" -->
## Challenges

---

## Challenges

- Drupal 8 Beta - read the source Luke.
- Webform? <span class="fragment">Nope!</span> <span class="fragment">Entity Form?</span> <span class="fragment">Kinda!</span>
- Field Collections? <span class="fragment">Nope!</span> <span class="fragment">DIY.</span>
- Conditional Fields? <span class="fragment">Nope!</span> <span class="fragment">DIY.</span>
- Field Groups? <span class="fragment">Nope!</span> <span class="fragment">DIY.</span>
- Path Auto? <span class="fragment">Nope!</span> <span class="fragment">DIY.</span>

---

## Challenges 
### EForm

- Patches:
  - Issue #2630226 array_flip warnings
  - Issue #2585841 PHP Fatal error with D8 Rc1
  - Issue #2574461 Declaration of submissionsPage should be compatible strict error
  - Issue #2511820 Wrong entity form submission form returned when form is accessed
  - Issue #2692497 Either remove or add permission to nuke "feature"
  - Issue #2611034 Make visibility of "View your previous submissions" configurable

---

## Challenges:
### DIY Field Collection

- Create a custom combi-field:
  - Create a FieldType Plugin, specify multiple columns in the schema
  - Create a FieldWidget Plugin with formElement that has multiple fields on it and for ease in a container e.g. fieldset
  - Implement massageFormValues to pull the values out of the fieldset. Add in a base widget class to do this for all your custom combi-fields
- Pain:
  - Instances where some of the sub-fields are required and others are not

---

## Challenges:
### DIY Conditional Fields

- KISS: 
  - A field can have one parent controlling field
  - Visibility is dependent on whether parent is set to specific option (e.g. Yes or No)
  - Number of levels can be more than one (recursion ftw)
  - Radios and select boxes only
- Add in classes that identify parent and children and which options they react to
- JS does the rest
- Pain:
  - Need to check visibility on server to determine if form is complete in validation

---

## Challenges: 
### DIY Field Groups

- <del>Bunch of weird form array alterations in hook_alter</del>
- Field Group module was released
- It does the Right Thing&trade;, with drag and drop in the form display and does it's magic in template preprocess and leaves the form_array alone

---

## Challenges:
### DIY Path Auto

- Wouldn't it be nice to have /scholarship/{category 1}/{category 2}/{title}?
  - Service path.alias_storage and service path.alias_manager are your friends
  - Hook node_insert, node_update
  - Handle path collisions
- Add in custom routes for {eform_type}/apply, {eform_type}/save, {eform_type}/confirm, {eform_type}/submit

---

## Not a Challenge

- Drupal goodness: Content management, performance, UI, responsive
- List Scholarships: Views
- Search Scholarships: Views Search (search module not required)
- Themeing: UA D8 Theme (Styleguide)

---

<!-- .slide: data-background="images/ikea.png" -->
## Some Assembly Required

---

## Some Assembly Required

- Some common fields across forms? 
  - Pull in common settings based on machine_name prefix, optional override
  - Not a good solution - needs work.
- Middleware integration (UA Middleware Service)
- Create PDF of the scholarship application on server
- Private file management (script to move files)
- Disable inbuilt search (/search) with Routes RouteSubscriber -> alterRoutes
- Add extra permissions to arbitrary routes using a RouteSubscriber and a custom AccessInterface -> accessCheck

---

## Some Assembly Required

- Autocomplete on search (typeahead)
- Fields with cardinality != 1:
  - Say no to unlimited. Use some sane limits: set to 5 for files, 3 for other items
  - Remove the widget table/dragging/ordering stuff (confusing and ugly)
  - Hide extras with JS / CSS and add in a _remove item_ button
  - Do some smart checking around combi-fields and what values are / are not required for the form to be complete

---

<!-- .slide: data-background="images/dogfood.jpg" -->
## Dogfooding

---

## Dogfooding
### Creating forms, well, sucks. Make it easier:

- Create a base form with the common fields
- Clone an existing form
- Wizard for creating fields: Field Templates
- Synchronise form displays on save by default
- Mass Creation: Import Forms via CSV
- Manage conditional fields on form display

---

<!-- .slide: data-background="images/modular.jpg" -->
## Reusable Modules

---

## Reusable Modules

(to be cleaned up a bit and put in drupal.org sandboxes)

### Limit Textarea Characters
- Specify limit on form display widget settings
- Use a global limit or per instance
- Creates a live count down of characters left underneath textarea
- Uses jQuery limit plugin

### Long Field Labels
- Specify on field settings page so will show on all form / view displays

---

## Reusable Modules

### Clone EForm
- Adds an action to the operations on the eform listing overview
- Specify title and new machine name

### Basic Conditional Fields
- Specify on the form display
- Gives you a select box with a list of fields to choose parent from and specify value to trigger on

---

## Reusable Modules

### Sync EForm Form Displays
- Setting per EForm to sync form displays on save

### Form Display Machine Names
- Useful when you have fields with the same labels
- Happens when using field_group or conditional fields

---

## Reusable Modules 

### Field Templates
- Quickly create a field for a form with common settings across storage, instance, widget, formatter
- Specify using Yaml, yeah!
- Create a new field by specifying template, label, machine_name (optional), description / help. Done!

### Import CSV to EForm
- Needs refactoring to use plugins / field templates and possibly migrate csv

---

## Questions?

---

## Thank you!

Michael Welford

<michael.welford@adelaide.edu.au>
