ng-Fx    [![Build Status](https://travis-ci.org/Hendrixer/ngFx.svg?branch=master)](https://travis-ci.org/Hendrixer/ng-Fx)   <img src="http://img.shields.io/badge/Built%20with-Gulp-red.svg" />
===============

### This is fork of Hendrixer's ngFx module (https://github.com/Hendrixer/ngFx)
#### I forked from it's original repo to make a light version of ngFx, that doesn't include TweenMax and ngAnimate.
#### I prefer to include them separately.

## Interactive Demo
Preview the goodness at [hendrixer.github.io](https://hendrixer.github.io/).

## Dependencies
+ Angular.js (1.2+)

## Downloading
1. The best way to install ng-Fx is to use bower (this is a light version of ngFx, so you'll need to include angular-animate and TweenMax manually, or from bower).
    + ```bower install ngFx --save```
2. Or, from this repo
  + you'll need the main file in ```dist/ngFx.js```

## Installing
1. Include ```ngFx.js``` into your html. 
  + __Note:__ ```ngFx``` bundles ```ngAnimate``` and ```GSAP``` into one file
2. Include the dependencies into your angular app,  ```ngFx```
```javascript
angular.module('myApp', ['ngFx'])
```
##Using
###Animations
+ All animations are used with ngAnimate. So you can apply them to...
  + ng-hide / ng-show
  + ng-include
  + ng-if
  + ng-view
  + ui-view (if you're using ui.router)
  + ng-switch
  + ng-class
  + ng-repeat
+ Adding the animations are as simple as adding a css class. ngFx uses the ```'fx'``` name space. Here's an example using a fade animation. The list items will enter / leave / and move with the 'fade-down' animation. __Note that ng-repeat will not trigger animations upon page load, the collection you are iterating over must be empty at first then populated, you can achieve this with a simple timeout or some other async operation.__

```javascript
angular.module('foodApp', ['ngFx'])
.controller('FoodController', function($scope, $timeout){
  $timeout(function(){
    $scope.foods = ['apple', 'muffin', 'chips'];
  }, 100);
});
```
```html
<ul ng-controller="FoodController">
  <li class='fx-fade-down' ng-repeat="food in foods">
    {{ food }}
  </li>
</ul>
```
###Easings
+ You can also add a different easing to any animation you want. It is as easy as adding an animation, just use a CSS class. Building on the previous example, you can just add ```fx-easing-your easing here```
```html
<ul ng-controller="FoodController">
  <li class='fx-fade-down fx-easing-bounce' ng-repeat="food in foods">
    {{ food }}
  </li>
</ul>
```
###Speed
+ Adjusting the speed in the ng-fx is a snap too! The speeds at which your animations enter and leave your app are totally up to you. You just have to add a CSS class. ```fx-speed-your speed in milliseconds```. All animations have their own default speed if not provided by you. There are __no predefined classes for speeds__. Any speed (in ms) can be accepted.
```html
<ul ng-controller="FoodController">
  <li class='fx-fade-down fx-easing-bounce fx-speed-800' ng-repeat="food in foods">
    {{ food }}
  </li>
</ul>
```
###Events
+ Animations will emit events to your app when they have finished. You can listen to these events in your controllers and directives to perform other things. When an animation is complete the event will look like so ' [animation name] :[enter or leave]', for example 'fade-down:enter' or 'zoom-up:leave'. You just have to add the CSS class ```fx-trigger``` to an animated element.
```javascript
angular.module('myApp', ['ngFx'])
.controller('FoodController', function($scope, $timeout){
  $timeout(function(){
    $scope.foods = ['apple', 'muffin', 'chips'];
  }, 100);
})
.directive('goAway', function($animate){
  function link(scope, element){
    scope.$on('fade-down:enter', function(){
      $animate.leave(element);
    });
  }
  return {
    restrict: 'A',
    link: link
  };
});
```
```html
<div ng-controller="FoodController">
  <h1 go-away class='fx-zoom-up'> This will zoom out when the fade animation is done</h1>
  <ul>
    <li class='fx-fade-down fx-easing-bounce fx-speed-800 fx-trigger' ng-repeat="food in foods">
      {{ food }}
    </li>
  </ul>
</div>
```
###List of animations and ease types
+ [Animations](https://github.com/Hendrixer/ng-Fx/blob/master/animationList.txt)
+ [Ease Types](https://github.com/Hendrixer/ng-Fx/blob/master/easingList.txt)

##What's next
+ More animations
+ More flexibility
+ Easy api to create your own animations
+ Events triggered are unique to element and not just animation type

##Contributing
1. Fork it
2. Clone your fork
3. Create new branch
4. Make changes
5. Make test and check test
6. Build it, run ```gulp``` and the files will be linted, concatenated, and minified
7. Push to new branch on your forked repo
8. Pull request from your branch to ngFx master

###Format for pull request
+ Pretty standard
  + in your commit message; ```(type) message [issue # closed]```
    + ```(bug) killed that bug, closes #45```
  + if you're submitting new animations:
    + ```(new fx) added 3d rotation animation ```
+ Submit issues as you see them. There are probably better, faster, easier ways to achieve what ngFx is designed to do so.

###Testing
+ ngFx uses Karma + Jasmine + Travis for unit and ci
+ Make sure you didn't break anything
  + run ```karma start``` to test in Chrome with karma
+ Features will not be accepted without specs created for them
+ Run ```gulp``` and all the source files will be watched and concatenated
+ Open the ```index.html``` and use the test app as a playground
