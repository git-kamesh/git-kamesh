+++ 
title = "Detecting & Adapting to Device Types with Custom Hooks"
description = "In this blog post, we'll explore a custom React hook that allows you to detect the device type and adapt your UI based on the screen width."
date = "2023-11-04"
author = "Kamesh Sethupathi"
tags = ["react", "react hook", "snippet", "javascript", "privacy"]
+++

- In the world of web development, creating [responsive and adaptive user interfaces](https://www.smashingmagazine.com/2011/01/guidelines-for-responsive-web-design/) is crucial. Ensuring that your website or web application looks & functions well across various devices, from mobile phones to desktop computers, is a basic requirement. 
- One way to achieve this is by detecting the device type and adjusting your content or styles accordingly. 
- In this blog post, we'll explore a custom [React](https://react.dev/) hook that allows you to detect the device type and adapt your UI based on the screen width.

### useDeviceType Hook

Below `useDeviceType` [hook](https://react.dev/reference/react/hooks) aims to determine the device type based on the screen width using [matchMedia](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia).

```js
import { useState, useEffect } from 'react';


const useDeviceType = () => {
  const [deviceType, setDeviceType] = useState(null);

  // Define constants for media queries
  const MEDIA_QUERIES = {
    MOBILE: '(max-width: 767px)',
    TABLET: '(min-width: 768px) and (max-width: 1023px)',
    LAPTOP: '(min-width: 1024px) and (max-width: 1439px)',
    DESKTOP: '(min-width: 1440px)',
  };

  useEffect(() => {
    const mediaQuery = window.matchMedia(
      `${MEDIA_QUERIES.MOBILE}, ${MEDIA_QUERIES.TABLET}, ${MEDIA_QUERIES.LAPTOP}, ${MEDIA_QUERIES.DESKTOP}`
    );

    const handleMediaChange = (mediaQueryList) => {
      if (mediaQueryList.matches) {
        if (mediaQueryList.media.includes(MEDIA_QUERIES.MOBILE)) {
          setDeviceType('mobile');
        } else if (mediaQueryList.media.includes(MEDIA_QUERIES.TABLET)) {
          setDeviceType('tablet');
        } else if (mediaQueryList.media.includes(MEDIA_QUERIES.LAPTOP)) {
          setDeviceType('laptop');
        } else if (mediaQueryList.media.includes(MEDIA_QUERIES.DESKTOP)) {
          setDeviceType('desktop');
        }
      }
    };

    // Initial device type detection
    handleMediaChange(mediaQuery);

    // Add event listener to update device type when the media query changes
    mediaQuery.addListener(handleMediaChange);

    return () => {
      // Cleanup: remove the event listener when the component unmounts
      mediaQuery.removeListener(handleMediaChange);
    };
  }, []);

  return deviceType;
};

export default useDeviceType;
```

### Usage 

```js
import React from 'react';
import useDeviceType from './useDeviceType';

function MyComponent() {
  const deviceType = useDeviceType();

  return (
    <div>
      <p>Device Type: {deviceType}</p>
      {/* Render different content based on the device type */}
      {deviceType === 'mobile' && <p>This is mobile content</p>}
      {deviceType === 'tablet' && <p>This is tablet content</p>}
      {deviceType === 'laptop' && <p>This is laptop content</p>}
      {deviceType === 'desktop' && <p>This is desktop content</p>}
    </div>
  );
}

export default MyComponent;
```


{{< codepen id="zYeoLZm" >}}

---

{{< include "reachout.md" >}}