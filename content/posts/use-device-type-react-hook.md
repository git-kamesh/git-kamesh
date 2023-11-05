+++ 
title = "Detecting and Adapting to Device Types in React with Custom Hooks"
description = "In this blog post, we'll explore a custom React hook that allows you to detect the device type and adapt your UI based on the screen width."
date = "2023-11-04"
author = "Kamesh Sethupathi"
tags = ["react", "react hook", "snippet", "javascript", "privacy"]
+++

- In the world of web development, creating responsive and adaptive user interfaces is crucial. Ensuring that your website or web application looks & functions well across various devices, from mobile phones to desktop computers, is a basic requirement. One way to achieve this is by detecting the device type and adjusting your content or styles accordingly. 
- In this blog post, we'll explore a custom React hook that allows you to detect the device type and adapt your UI based on the screen width.

### useDeviceType Hook

Below `useDeviceType` hook aims to determine the device type based on the screen width and provide that information to your components.

```js
import { useState, useEffect } from 'react';

const useDeviceType = () => {
  const [deviceType, setDeviceType] = useState(null);

  useEffect(() => {
    const handleResize = () => {
      const screenWidth = window.innerWidth;

      if (screenWidth < 768) {
        setDeviceType('mobile');
      } else if (screenWidth >= 768 && screenWidth < 1024) {
        setDeviceType('tablet');
      } else if (screenWidth >= 1024 && screenWidth < 1440) {
        setDeviceType('laptop');
      } else {
        setDeviceType('desktop');
      }
    };

    // Initial device type detection
    handleResize();

    // Add event listener to update device type on window resize
    window.addEventListener('resize', handleResize);

    return () => {
      // Cleanup: remove the event listener when the component unmounts
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return deviceType;
};

export default useDeviceType;
```

### Measures
- To ensure that the device type is updated dynamically as the user resizes their browser window, an event listener is added to the resize event
- To prevent memory leaks and ensure that the event listener is removed when the component using the useDeviceType hook is unmounted, helps maintain good performance and memory management.

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