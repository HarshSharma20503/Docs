# Installation

Install package

```console
npm i @chakra-ui/react @emotion/react @emotion/styled framer-motion
```

After installing Chakra UI, you need to set up the `ChakraProvider` at the root of your application. This can be either in your `index.jsx`, `index.tsx` or `App.jsx` depending on the framework you use.

```javascript
import * as React from 'react'

// 1. import `ChakraProvider` component
import { ChakraProvider } from '@chakra-ui/react'

function App() {
  // 2. Wrap ChakraProvider at the root of your app
  return (
    <ChakraProvider>
      <TheRestOfYourApplication />
    </ChakraProvider>
  )
}
```

Now you can start using Chakra UI Components

---
