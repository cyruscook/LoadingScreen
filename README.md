# LoadingScreen

A loading screen in 3 lines of code. Completely automated - all you need to do is add the code.

The first line is CSS, second is HTML and third is JS.

## Table of Contents

[How to use](#how-to-use)

[Customisation](#customisation)

&nbsp;&nbsp;&nbsp;&nbsp;[Delay](#delay)

&nbsp;&nbsp;&nbsp;&nbsp;[Changing the Message](#changing-the-message)

&nbsp;&nbsp;&nbsp;&nbsp;[Showing the loading screen multiple times](#showing-the-loading-screen-multiple-times)

&nbsp;&nbsp;&nbsp;&nbsp;[Removing the scrollbar](#removing-the-scrollbar)

[How it works](#how-it-works)

## How to use

Copy all of the code in [src/plain.html](https://raw.githubusercontent.com/cyruscook/LoadingScreen/master/src/plain.html) or [src/with_animation.html](https://raw.githubusercontent.com/cyruscook/LoadingScreen/master/src/with_animation.html) depending on whether you want a plain "Loading..." or an animation, and place it as close to the top of your website as is humanely possible. I would suggest immediately after the `<html>` tag. For optimal performance it should be before any external dependencies like Bootstrap / JQuery. This code is written in vanilla JavaScript so does not require them.

### Customisation

##### Delay

By default the Loading Screen will only be removed after 500 ms has passed, even if the page has already loaded. This creates a smoother experience than a white flash of the loading screen appearing and then instantly being removed. 

You can change this by changing every instance of `500` in the code to your own value. Setting this too high or too low will be annoying for users with fast connections.

##### Changing the Message

You can quite easily change the loading message simply by changing `Loading...` in the code. It is also possible to add an animation, such as a loading spinner. While I was going to include this, it would double the size of the code for this project. 

If you are going to add in an animation remember that there is a balance between speed and beauty, and most users would prefer a shorter loading time over a beautiful loading screen.

##### Showing the loading screen multiple times

The loading screen will completely delete itself and all of it's code once the page has loaded. If you want you could quite easily change it to only hide itself, allowing you to re-show it if your performing an ajax request for example.

To do this, find `document.getElementById("LoadingScreen").parentNode.removeChild(document.getElementById("LoadingScreen"))` and replace it with `document.getElementById("LoadingScreen").style.display = "none"`.

By adding the following two functions to your code you can use `LS.show()` and `LS.hide()`:
```javascript
var LS = {
	e:
		document.getElementById("LoadingScreen"),
	show:
		() => {
			LS.e.style.display = "";
		},
	hide: 
		() => {
			LS.e.style.display = "none";
		}
}
```

##### Removing the scrollbar

You might have noticed that the scrollbar is visible while the loading screen is on the screen. Here is how to fix this:

**Regular Loading Screen**

Change your body to:
```html
<body style="overflow: hidden;">
```

And change
```javascript
document.getElementById("LoadingScreen").parentNode.removeChild(document.getElementById("LoadingScreen"))
```
to
```javascript
document.getElementById("LoadingScreen").parentNode.removeChild(document.getElementById("LoadingScreen"));document.body.style.overflow = "";
```

**Retriggerable Loading Screen**

Change
```javascript
var LS = {
	e:
		document.getElementById("LoadingScreen"),
	show:
		() => {
			LS.e.style.display = "";
		},
	hide: 
		() => {
			LS.e.style.display = "none";
		}
}
```
to

```javascript
var LS = {
	e:
		document.getElementById("LoadingScreen"),
	show:
		() => {
			LS.e.style.display = "";
			document.body.style.overflow = "hidden";
			
		},
	hide: 
		() => {
			LS.e.style.display = "none";
			document.body.style.overflow = "";
		}
}
```

## How it works

The code, being placed at the very top of the page is the first thing to be executed. This renders the loading screen over everything else on the page. Once the loading screen has been rendered the browser will begin to render the rest of the page.

Once this is finished the browser will run the `pageshow` callback (supported by Chrome, Edge 11.0, Firefox, Opera and Safari 5.0).

When this callback runs the script checks if 500 ms have passed since the loading screen was added to the page. If so, we will remove the loading screen. If not we will set a timer that will remove the loading screen once 500 ms have passed.
