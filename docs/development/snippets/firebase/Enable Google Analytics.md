# Google Analytics Setup Guide for React Applications

⚠️ **Important**: Disable any ad blockers for Google Analytics to work properly.

## 1. Installation
```bash
npm install react-ga4
```

## 2. Initialize Analytics
In your `App.js` or main entry file:

```javascript
import ReactGA from 'react-ga4';

// Initialize GA4 with your measurement ID
ReactGA.initialize('G-XXXXXXXXXX');
```

## 3. Track Page Views
Add this component to handle route changes:

```javascript
import { useEffect } from 'react';
import { useLocation } from 'react-router-dom';
import ReactGA from 'react-ga4';

function PageViewTracker() {
  const location = useLocation();

  useEffect(() => {
    ReactGA.send({ hitType: "pageview", page: location.pathname });
  }, [location]);

  return null;
}

// Use in App.js
function App() {
  return (
    <Router>
      <PageViewTracker />
      {/* Your routes */}
    </Router>
  );
}
```

## 4. Track Custom Events
Create a utility function for event tracking:

```javascript
export const trackEvent = (
  name: string,
  parameters?: { [key: string]: any }
) => {
  ReactGA.event(name, parameters);
};

// Usage example
trackEvent('login_click', { method: 'google', userId: '123' });
```

## 5. Common Use Cases
```javascript
// Track button click
<button onClick={() => trackEvent('login_button_click', { method: 'email' })}>
  Login
</button>

// Track form submission
const handleSubmit = () => {
  trackEvent('form_submit', { 
    form_type: 'contact',
    form_id: 'contact-123'
  });
  // ... form handling logic
};
```

## Troubleshooting
1. Verify measurement ID is correct
2. Disable ad blockers
3. Check browser console for errors
4. Wait 24-48 hours for data to appear in GA dashboard