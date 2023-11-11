+++ 
title = "Boosting Website Responsiveness `with window.matchMedia`"
description = "Responsive web design is about ensuring your website looks and works well on different devices. It's essential for today's web. Traditionally, developers used the window width (window.innerWidth) to make sites responsive, but there's a better way: window.matchMedia. Let's see why."
date = "2023-11-10"
author = "Kamesh Sethupathi"
tags = ["react", "react hook", "snippet", "javascript", "privacy"]
+++

[Responsive web design](https://www.w3schools.com/html/html_responsive.asp) is about ensuring your website looks and works well on different devices. It's essential for today's web. Traditionally, developers used the window width ([window.innerWidth](https://developer.mozilla.org/en-US/docs/Web/API/Window/innerWidth)) to make sites responsive, but there's a better way: [window.matchMedia](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia). Let's see why.

### Traditional way using resize event and innerWidth

```js
const handleResize = () => {
  const windowWidth = window.innerWidth;

  if (windowWidth < 768) {
    // Adjust layout for mobile
  } else if (windowWidth >= 768 && windowWidth < 1024) {
    // Adjust layout for tablet
  } else if (windowWidth >= 1024 && windowWidth < 1440) {
    // Adjust layout for laptop
  } else {
    // Adjust layout for desktop
  }
};

// Initial adjustment
handleResize();

// Add event listener to handle window resizing
window.addEventListener('resize', handleResize);
```

In this code:
- We continuously check the window width with window.innerWidth.
- Depending on the window size, we apply different layout adjustments.
- We listen for the [resize event](https://developer.mozilla.org/en-US/docs/Web/API/Window/resize_event) and call handleResize whenever the window is resized.

### Better way through `window.matchMedia`

```js
const handleMediaChange = (mediaQueryList) => {
  if (mediaQueryList.matches) {
    // Apply styles or behavior when the media query matches.
  } else {
    // Revert styles or behavior when the media query no longer matches.
  }
};

const mediaQuery = window.matchMedia('(max-width: 767px)');
mediaQuery.addListener(handleMediaChange);

// Check it right away
handleMediaChange(mediaQuery);
```

With this code:
- We use window.matchMedia to observe media queries. In this example, we check if the screen width is less than 768 pixels.
- The handleMediaChange function applies or reverts styles or behavior when the media query matches or no longer matches.
- We add an event listener to react to changes in the media query, and we check it initially.