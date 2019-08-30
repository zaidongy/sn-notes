# CAAF Implementation

## Tables of Contents

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [CAAF Implementation](#caaf-implementation)
  - [Tables of Contents](#tables-of-contents)
  - [The Widget](#the-widget)
    - [The HTML Code](#the-html-code)
      - [Bootstrap Grid System](#bootstrap-grid-system)
      - [Bootstrap Form controls](#bootstrap-form-controls)
      - [Combining the two concepts](#combining-the-two-concepts)
      - [AngularJS Providers](#angularjs-providers)
      - [AngularJS Form Validations](#angularjs-form-validations)
      - [Select Box Dropdown Options](#select-box-dropdown-options)
      - [Submission Feedback](#submission-feedback)
    - [CSS - Styling the widget](#css-styling-the-widget)
    - [Client Script - Controller](#client-script-controller)
      - [Form initilization](#form-initilization)
      - [Form Submission Handler](#form-submission-handler)
    - [The server script](#the-server-script)
      - [Initilizaing the dropdowns](#initilizaing-the-dropdowns)
      - [Demo Data for testing](#demo-data-for-testing)
      - [Processing the `c.server.update()`](#processing-the-cserverupdate)
  - [Dependencies](#dependencies)
    - [Bootstrap Select](#bootstrap-select)
    - [JQuery UI](#jquery-ui)
  - [Author](#author)

<!-- /code_chunk_output -->

## The Widget

The CAAF form is implemented with a single widget. Every Service Portal widget has four main components:

1. HTML - Stucture of the widget
2. CSS - styling of the widget
3. Client Script (controller) - controls behavior for the widget
4. Server Script - Interacts with the server

There are other components such as link function or dependencies that allow for more advanced functionalities, but we won't need them here.

Service Portal's technology stack uses 2 main pieces:

1. AngularJS 1.5.11
2. Bootstrap 3.3.6

AngularJS is a front end framework that allows data binding to easily connect the server data with our client data.

Bootstrap is a collection of CSS classes that allow the web page to be styled in a consistent manner.

### The HTML Code

The HTML code defines the structure of the widget. Using the Bootstrap grid system, we are able to easily contorl where components should go and the boostrap classes will style them for us. That combined with the Bootstrap form controls will create a consistnent user experience.

#### Bootstrap Grid System

The structure of the grid system is:

1. Start defining a container
2. Container should have a row inside
3. Row should be followed by a combination of 12 columns
4. Inside each combination of columns, you put the element you're interested in

Example: This code will generate three elements in the same row spaced automatically by bootstrap

```html
<div class="container">
  <div class="row">
    <div class="col-sm-4">
      First element for this row!
    </div>
    <div class="col-sm-4">
      Second element for this row!
    </div>
    <div class="col-sm-4">
      Third element for this row!
    </div>
  </div>
</div>
```

#### Bootstrap Form controls

Here is the basic structure of a form with Bootstrap

```html
<form name="caaf">
  <div class="form-group">
    <label for="first_name">First Name</label>
    <input type="text" class="form-control" id="first_name name="first_name"
    placeholder="First Name" ng-model="c.data.first_name" ng-required="true" />
    <label for="last_name">Last Name</label>
    <input type="text" class="form-control" id="last_name name="last_name"
    placeholder="Last Name" ng-model="c.data.last_name" ng-required="true" />
  </div>
</form>
```

1. We start with the `form` element
2. Then for each group of inputs we can wrap them in a `form-group`
3. Then we can create our `label` and `input` tags, be sure to give the input the form-control class

#### Combining the two concepts

When we combine grid and the form stcutures, we are able to place the fields in any place and row we want. As long as we follow the two sets of rules above, the bootstrap classes will take care of the rest.

#### AngularJS Providers

The benefit of AngularJS is to allow you to dynamcially assign values to certain proeprties in the HTML code. This simiplifies things alot as traditionally you'd need to interact with the DOM to get the desired effects. However angularjs will take care of the DOM for you in a consistent and predictable manner.

Take this code for instance:

```html
<input
  type="text"
  class="form-control"
  id="first_name"
  name="first_name"
  placeholder="First Name"
  ng-model="c.data.first_name"
  ng-required="true"
/>
<small
  class="errormsg"
  ng-show="caaf.first_name.$invalid && caaf.first_name.$dirty"
  >This field is required</small
>
```

The `ng-show` angular directive takes an expression which can be evaluated to get the desired results of when to show this element and when to hide it. This expression is saying only show this message when the first_name field is invalid and first_name field has been touched (\$dirty), since we don't want to show the error message when the form loads before given user a chance to enter a value.

`$invalid` and `$dirty` are input states and they have a boolean(true/false) value, we'll get more into this in the validation section.

**Data Binding**
When `ng-model` is specified with an input, the value of that input is bounded from the View and the Controller (for more info look up Model View Controller - MVC). So if you change that field in the controller (client script), it will automatically be reflected in the view (html) and vice versa. Every input should have this so the controller can make API calls to the server with the latest input data.

Here are some common providers used in the caaf form:

- `ng-model` - Enables two-way data binding to the variable provided, every input should have this
- `ng-model-option` - options to specify when the bounded variable should update
- `ng-show` - Show if expression is true, hide if false. This hides via css and is not removed from the DOM
- `ng-if` - Similar to `ng-show`, but if the expression is false, the element and all children will be removed from the DOM. When is becomes true again, all elements will need to be re-initalized. Don't use this if you need to preserve data
- `ng-repeat` - repeats the current element based on number of elements in the expression
- `ng-submit` - defines which function to call when the form is submitted
- `ng-required` - field will be required if true, used for validations
- `ng-pattern` - Defines the regex pattern the input should be, used for validations

#### AngularJS Form Validations

As mentioned before, AngularJS has validators built into framework. To use this awesome feature, we need to turn off the browser validation since they behave differently depending on the browser, we do this by specifying `novalidate` in our form property.

```html
<form
  name="caaf"
  class="css-form"
  novalidate
  ng-submit="c.formSubmitted(caaf.$valid)"
></form>
```

**Mandatory Fields**
Consider the following scenario: we want the CPO Office location field to only be mandatory when the CPO radio option is selected.

```html
<!-- Conditional Validators -->
<select
  name="cpo_office_location"
  id="cpo_office_location"
  class="form-control selectpicker"
  data-live-search="true"
  ng-model="c.data.cpo_office"
  ng-required="c.data.insurance_plan == 'cpo'"
>
  <option></option>
  <option ng-repeat="x in c.data.office_location_list">{{::x}}</option>
</select>
```

`c.data.insurance_plan` is data bounded to the radio buttons, so whenever the value of the radio button changes, the value of this variable would also change. And then we can use this variable in `ng-required` to specify that the select field is only required when `c.data.insurance_plan` is equal to 'cpo'.

**Phone Number** fields use the `ng-pattern="ph_number"` to validate the pattern of the input. In below example, the `ph_number` variable is initialized in the client controller

```html
<input
  type="text"
  name="primary_phone"
  id="primary_phone"
  class="form-control"
  ng-pattern="ph_numbr"
  placeholder="123-456-7890"
  ng-model="c.data.primary_phone"
  required
/>
```

**Email** fields are specified using the `type="email"` property, and AngularJS will validate using the **name@domain** email format.

```html
<input
  type="email"
  class="form-control"
  id="sp_email"
  placeholder="user@example.com"
  ng-model="c.data.sp.email"
  required
/>
```

**Form and Input Field States**
Now that we defined all the rules for each field, how do we know if a field a valid or not?

AngularJS attaches states to the form and each field on the form. The value of these state fields will automatically update. These validation states begin with a $ ($invalid, $valid, $dirty, etc...).

Input field states

- `$untouched` - The field has not been touched yet
- `$touched` - The field has been touched
- `$pristine` - The field has not been modified yet
- `$dirty` - The field has been modified
- `$invalid` - The field content is not valid
- `$valid` - The field content is valid

Form states

- `$pristine` - No fields have been modified yet
- `$dirty` - One or more have been modified
- `$invalid` - The form content is not valid
- `$valid` - The form content is valid
- `$submitted` - The form is submitted

Example:
Here we pass the state of the form into the `c.formSubmitted` function which gets called in the controller. The controller can then process what to do if the form is not valid, in our case we set focus on the first invalid element of the form to direct the user where the issue is.

```html
<!-- HTML -->
<form name="caaf" novalidate ng-submit="c.formSubmitted(caaf.$valid)"></form>
```

```javascript
// Controller (client script)
c.formSubmitted = function (valid) {
    c.data.spomerror = !(c.data.spchecked || c.data.omchecked);
    if (!valid) {
        // console.log($scope.caaf.$error.required);
        angular.element('input.ng-invalid, select.ng-invalid').first().focus();
    }
```

#### Select Box Dropdown Options

The options for these dropdown are defined in the `u_caaf_data` table in the platform. There are 4 types of data stored in this table, and can easily be scaled to more.

1. Office Location
2. Specialty
3. CPO Role
4. PPO Role

The server script function `getCaafData()` queries the table and stores the values in these variables

```js
data.office_location_list = getCaafData("Office Location");
data.specialty_list = getCaafData("Specialty");
data.cpo_role_list = getCaafData("CPO Role");
data.ppo_role_list = getCaafData("PPO Role");
```

Then in the View (HTML), we can display the contents of these variables in the dropdown with `ng-repeat`. One thing to note here is I used `{{ ::x }}` instead of `{{ x }}` to one time bind the variable since we're just displaying the values, so there's no need for two way binding, this improves performance.

```html
<select
  name="cpo_access_role"
  id="cpo_access_role"
  class="form-control selectpicker"
  data-live-search="true"
  ng-model="c.data.cpo_role"
  ng-required="c.data.insurance_plan == 'cpo'"
>
  <option></option>
  <option ng-repeat="x in c.data.cpo_role_list">{{ ::x }}</option>
</select>
```

The only dropdown hard coded to the view is the `state` since it's not going to change anytime soon... Unless Trump starts invading other countries.

#### Submission Feedback

The feedback is handled by hiding the input fields and displaying a success or error message based on whether the server call succeeds. I have personally never seen it fail but it's good to consider all cases and build feedback that will reflect it.

**The feedback messages** are shown in this code block, `formSubmitted` and `formSubmitionSuccess` are updated in the controller after the user hits the submit button. We use `ng-if` with opposite conditions to make sure either success message or failure message show up, but not both.

```html
<!-- Form submission feedback messages -->
<div class="row" ng-if="formSubmitted">
  <div class="col-sm-12">
    <div class="text-center">
      <h3 id="success_message" ng-if="formSubmitionSuccess">
        Success! Your tracking number is: <strong>{{ c.data.number }}</strong
        ><br />Please save this number for your reference
      </h3>
      <h3 id="failure_message" ng-if="!formSubmitionSuccess">
        Failure! Please contact <strong>example-email@cshs.org</strong> with the
        current time to troubleshoot
      </h3>
    </div>
  </div>
</div>
```

**To hide all the input fields**, we wrap everything except the feedback messages in this div, and we use ng-show to hide it after the form is submitted.

```html
<!-- Fields Section -->
<div ng-show="!formSubmitted"></div>
```

### CSS - Styling the widget

Even though Bootstrap provides us with many classes to consistently style the components, some customs styles are still required to get the elements to look the exact way you want, and we do that in the CSS section of the widget. Service Portal actually supports SCSS which allows you to use variables in the style sheet, but we don't that here in this widget, maybe we can add it as a future enhancement to make the CSS more modular.

Most of these are pretty self explanatory, I'll just list the classes here and where it's used, and you can look at the source code to see the stylings applied

- `form-title` - style for the title (CAAF)
- `section-title` - style for the section title (INSTRUCTION, ACCOUNT HOLDER INFORMATION, ETT..)
- `btn-submit` - style for the submit button
- `errormsg` - style of the error messages when validation fails
- `.required label:after, label.required:after` - these are added to the label or the div that wraps the label add the "\*" to indictae required fields
- `css-form input.ng-invalid.ng-touched` - highlight input field red when validation fails
- `css-form input.ng-valid.ng-touched` - highlight input field green when validation passes

### Client Script - Controller

The controller can define how elements should behave when clicked on, and can modify variable values. Let's go through section by section.

#### Form initilization

```javascript
/* Angular widget controller */
var c = this;
c.data.agent = window.navigator.userAgent;

// setup some components that uses 3rd party libraries
//Need to put it in setTimeout because the DOM is not ready at runtime
setTimeout(function() {
  $("select").selectpicker();
  // $('select').selectpicker('refresh');
}, 0);
$("#date_of_birth").datepicker();
$("#date_of_birth").datepicker("option", "changeYear", true);
$("#date_of_birth").datepicker("option", "changeMonth", true);
$("#date_of_birth").datepicker("option", "yearRange", "-100:+0");

$scope.formSubmitted = false;
$scope.ph_numbr = /\+?1?\s*\(?-*\.*(\d{3})\)?\.*-*\s*(\d{3})\.*-*\s*(\d{4})$/;
c.data.spomerror = false;
```

`c` is a reference to this widget, so we always use `c.data.variable` name instead of `this` to simplify things since `this` can change depending on the scope you're calling it.

`c.data.agent` gets the current browser and version the user is using to view the form. This helps us debug to see if an issue is related to the browser.

`setTimeout(function () { $('select').selectpicker(); }, 0);` this allows the select picker to be initialized correctly Tje library needs the DOM to be completely loaded before the `ng-repeat` can populate the dropdown, we set a timeout here.

`$("#date_of_birth").datepicker();` initializes the datepicker, and the options for that field

`$scope.formSubmitted = false;` this initializes the `formSubmitted` variable in the HTML code. By default the form is not submitted

`$scope.ph_numbr` defines the phone number regex used for `ng-pattern` in the HTML.

`c.data.spomerror = false` controls whether either Office Manager or Sponsoring physician is checked. Checkboxes are harder to validate and we need use this variable to defines if either checkbox is selected.

#### Form Submission Handler

When the form is submitted, `c.formSubmitted` is called. The code is self documented, but bascially it checks to see if the form is valid, it not then focuses on the first invalid element. THen it checks to see if the Sponsor information is selected, if not then it sets the `c.data.spomerror` flag to true, which shows an error message in the HTML. Then it checks the DOB again just in case since we had some issues previously with IE not filling in the DOB. Then finally after all these checks, we submit the form by calling `c.server.update()` which takes a callback function as a parameter. The callback sets flag to display success or failure messages.

```javascript
c.formSubmitted = function(valid) {
  c.data.spomerror = !(c.data.spchecked || c.data.omchecked);
  // Check if the form is valid
  if (!valid) {
    // console.log($scope.caaf.$error.required);
    angular
      .element("input.ng-invalid, select.ng-invalid")
      .first()
      .focus();
  }
  // Check if at least one sponsor information is provided
  else if (c.data.spomerror) {
    angular.element("#spcheck").focus();
  }
  // Double check DOB since IE is buggy
  else if (c.data.date_of_birth == "" || c.data.date_of_birth == undefined) {
    angular.element("#date_of_birth").focus();
  }
  // finally make the Server call to submit the ticket
  else {
    $("#FSsubmit").attr("disabled", true);
    $("#FSsubmit").html("Submitting...");

    c.server.update().then(
      function(result) {
        console.log(result);
        $scope.formSubmitted = true;
        $scope.formSubmitionSuccess = true;
      },
      function(err) {
        $scope.formSubmitted = true;
        $scope.formSubmitionSuccess = false;
      }
    );
  }

  // Future enhancement - redirect to new page for better feedback
  // Need to inject $location service in controller parameter to use angular's $location service
  // $location.url("/cs_public?id=caaf_submitted");
};
```

### The server script

The server script has two funtions. Initialize variables from the server to display them in the view, and also process `c.server.update()` with the data object as input. Let's walk through it.

#### Initilizaing the dropdowns

As mentioned in one of the previous sections, this block gets the data from `u_caaf_data` to populate the dropdowns.

```javascript
data.office_location_list = getCaafData("Office Location");
data.specialty_list = getCaafData("Specialty");
data.cpo_role_list = getCaafData("CPO Role");
data.ppo_role_list = getCaafData("PPO Role");

function getCaafData(dataPoint) {
  var returnArr = [];
  var gr = new GlideRecord("u_caaf_data");
  gr.addQuery("u_type", dataPoint);
  gr.orderBy("u_sequence");
  gr.query();
  while (gr.next()) {
    returnArr.push(gr.getDisplayValue());
  }
  return returnArr;
}
```

#### Demo Data for testing

```javascript
var testing = gs.getProperty("instance_name") != "<prod-instance>";
```

If the instance is not PROD, then testing mode is enabled, which fills demo data into each field, this saves a lot of time in the development phase.

#### Processing the `c.server.update()`

When `c.server.update()` is called from the controller, `c.data` is copied into `input` object in the server script. We then use the `input` data received to build an object that passes into the `CartJS` API to order the CAAF catalog item.

```javascript
if (input) {
        // console.log(input);

        //build the cart object
        var caafitem = {
            sysparm_id: "c411af46dbf82b0074c99447db96193e", // caaf
            variables: {
                u_user_agent: input.agent,
                last_name: input.last_name,
                middle_initial: input.mi,
                first_name: input.first_name,
                ...
            }
        };

        // Order the CAAF Item
        var cart = new sn_sc.CartJS();
        var cartresult = cart.orderNow(caafitem);
        data.number = cartresult.request_number;
    }
```

Finally we set `data.number` to the number that was return from the order. And since `data.number` is data bounded, we can display the number in the feedback message of the HTML.

```html
<!-- Feedback Message -->
<h3 id="success_message" ng-if="formSubmitionSuccess">
  Success! Your tracking number is: <strong>{{ c.data.number }}</strong
  ><br />Please save this number for your reference
</h3>
```

## Dependencies

We have two dependencies in this widget, they are defined in the "Dependencies" related list at the bottom of widget record (platform view, not widget editor).

1. Bootstrap-Select
2. JQuery UI

### Bootstrap Select

Bootstrap select is what allows us to have the nice select box dropdowns with searchable options
**[Bootstrap Select Docs](https://developer.snapappointments.com/bootstrap-select/examples/)**

### JQuery UI

JQuery UI allows us to use the datepicker for the Date of Birth field. Since the `type=date` is supported on chrome and not IE, we needed another way to initilized the datepicker.
**[JQuery UI Date Picker Docs](https://api.jqueryui.com/datepicker/)**

## Author

Chris Yang - [zaidongy@gmail.com](mailto:zaidongy@gmail.com)
