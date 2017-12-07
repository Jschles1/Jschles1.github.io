---
layout: post
title:      "Styling React Components"
date:       2017-12-05 21:56:32 -0500
permalink:  styling_react_components
---


As I undertook the journey of learning how to code, I always enjoyed working on the Back-End side of things with Ruby more than the Front-End side. This was primarily because I disliked working with CSS, in particular dealing with positioning and floating of elements. However, I started to enjoy the front-end side of things more once I began working with JavaScript. Out of all the technologies I learned about at Flatiron, I ended up enjoying working with React more than anything else. The composabilty of components and the feeling of working with many moving parts made working on the Front-end side of things enjoyable again. Of couse, you still have to apply CSS styles to those components, but fortunately, there are many different ways to do so to fit the needs of your application. Here's a few ways to go about styling your React components.

**Create a stylesheet specifically for your component, and import it into your component file.**

This offers a good way of organizing CSS code, but the code in these files will still be scoped globally.

**Use inline styles.**

Inline styles involve defining your styles in JavaScript, inside the component's render function. In your return JSX code, use the style property to implement the styles you defined on the selected element.

```
...
render(
   const styles = {
	    color: 'red'
	 }
	 
	 return (
	    <p style={styles}>Hello World!</p>
	 )
)
```

* Use whenever you want to scope a style to a single element and not any other element in your application (or even in your component).
* Also can be used to dynamically change styles on state or prop changes by changing the styles object properties.
* One downside to this approach is that you cannot define pseudoselectors or media queries. Using a third party package called Radium can enable their use.

**Use CSS Modules.**

CSS Modules are a feature we can enable by editing the build configuration of the application to allow imported CSS files to be scoped only to the current component file. Here's how to implement them:

1 . Run the npm run eject command to eject from react scripts and enable build conifguration editing (This is a permanent and non-reversable action, so commit and push any changes before running the command).

2 . In the newly generated config folder, open the webpack.config.dev.js file, and scroll down to the css-loader section and change the options: section to look like the following:

```
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    modules: true,
    localIdentName: "[name]__[local]___[hash:base64:5]"  
  },
},
```

3 . Also enable modules in the accompanying webpack.config.prod.js file:

```
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    modules: true,
    minimize: true,
    sourceMap: true,
   },
},
```

Now CSS modules are enabled. Now, when importing a stylesheet into a component, you assign it to a local constant object. Then, you replace the className strings in your components with the constant, with the specific class selector added on to the constant as it's key. So if you had a stylesheet with a class selector of `.red` that made the text color red, you could implement it like so:

```
import React, { Component } from 'react';
import styles from './stylesheet.css';

class App extends Component {
  render() {
	  return (
		  <p className={styles.red}>Hello World!</p>
		)
	}
}
```

So, as you can see here, the main benefit of CSS modules is having the ability to have CSS classes imported as a JavaScript object, storing the class names as property values.

In the end, you have to choose which approach is best when styling your React components. Each approach has its strengths and weaknesses. If you use any other way to implement CSS classes in your components, feel free to reach out.











