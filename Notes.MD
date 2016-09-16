## Models

* model
* valueMoment
* view.moment - This appears to be a volatile variable which is mutated as the view is loaded/navigated.
* view.value - a Date object
* value - UNUSED ??? - referenced by a watcher at line 624.

## Functions, usage, etc

* $scope.momentToDate (161) - sets view.value
* $scope.valueUpdate - sets unused scope.value (probably supposed to set view.value). Replace with `view.update`?
* $scope.isValidMoment - This should probably check moment.isMoment() instead of angular.isDefined()

* $scope.limits
    * checkView - this could be more efficient, if there is a min/max limit hit we needlessly create two moments

* view.update - Sets view.value to Date of view.moment
* view.render - set some navigation values and labels



## Watches

* model
    1. Create valueMoment from model (model is string)
    2. verify it is valid, and create view.moment, then copy that to the Date at view.value
* value
  1. format valueMoment for comparison to model
  2. set view.value - this looks like a recursion (it isn't because we exit if values are equal)
  3. Set model to the new string value
  4. Call "change" function (which shuold use ngChange). This is a simple scope.change(), it should be scope.change.call(this, ...)

* view.selected, view.value - render view

* minView, maxView - set the default view to load
* minDate, maxDate - set and check new limits
* locale - recreates min and max moments with new local. FIX: also created "model" moment with new local, but everywhere else model is a string. Is this supposed to be "valueMoment"?


## Proposal

* Keep "model" for backwards compatibility and for text input parsing. This value will need to be created if a moment is passed
  * This will require some sort of transclused/nesting when used on an input element, as the ng-model for the input will not be the same as the ng-model for the moment-picker
* New watcher on ngModel.$modelValue (probably the unix timestamp thereof (around line 623)


## Other concerns

* The picker view should be appended at the end of the body, not immediately following the directive. (done)
* The click handler is a bit clumsy. If the element already has focus, you can't open the picker.