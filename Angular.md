# Angular Stuff

### Controllers:
- No explicit constructor; constructor function is called at `.controller(...)`
- Any controller functions are equivalent to the constructor of 'normal' JavaScript
- `this` vs `$scope`:
  - When a view loads, `this` is set to the controller object
    - Example: `this.randFunc = function() {...}` will be created on the controller object, not `$scope`
    - Views (HTML) cannot access the `randFunc()` because it's not in the `$scope`
  - Example between `this` and `$scope`:
    - ```
      <div ng-controller="ParentCtrl">
          <a ng-click="logThisAndScope()">log "this" and $scope</a> - parent scope
          <div ng-controller="ChildCtrl">
              <a ng-click="logThisAndScope()">log "this" and $scope</a> - child scope
          </div>
      </div>
      ```
    - Within `ParentCtrl`:
      ```
      $scope.logThisAndScope = () => {
        console.log(this, $scope);
      }
      ```
    - First output will bethe same as the `$scope` and `this` are within the same controller
    - Second output will *not* be the same since when the function is called, the `$scope` is within `ChildCtrl` while the `this` object is attached to `ParentCtrl`
