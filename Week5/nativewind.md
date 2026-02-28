# Nativewind Installation Guide

## Installation with Expo

Nativewind works with Expo and provides a streamlined experience for React Native development.

### 1. Install Nativewind

Install `nativewind` and its peer dependencies:

```bash
npm install nativewind react-native-reanimated react-native-safe-area-context
npm install --dev tailwindcss@^3.4.17 prettier-plugin-tailwindcss@^0.5.11 babel-preset-expo
```

### 2. Setup Tailwind CSS

Run `npx tailwindcss init` to create a `tailwind.config.js` file.

Add the paths to all of your component files in your tailwind.config.js file:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  // NOTE: Update this to include the paths to all files that contain Nativewind classes.
  content: ['./index.ts', './src/**/*.{js,jsx,ts,tsx}'],
  presets: [require('nativewind/preset')],
  theme: {
    extend: {
      borderWidth: {
        hairline: '0.5px',
      },
    },
  },
  plugins: [],
};
```

Create a CSS file (global.css) and add the Tailwind directives:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
  --card: 0 0% 100%;
  --card-foreground: 222.2 84% 4.9%;
  --popover: 0 0% 100%;
  --popover-foreground: 222.2 84% 4.9%;
  --primary: 221.2 83.2% 53.3%;
  --primary-foreground: 210 40% 98%;
  --secondary: 210 40% 96%;
  --secondary-foreground: 222.2 84% 4.9%;
  --muted: 210 40% 96%;
  --muted-foreground: 215.4 16.3% 46.9%;
  --accent: 210 40% 96%;
  --accent-foreground: 222.2 84% 4.9%;
  --destructive: 0 84.2% 60.2%;
  --destructive-foreground: 210 40% 98%;
  --border: 214.3 31.8% 91.4%;
  --input: 214.3 31.8% 91.4%;
  --ring: 221.2 83.2% 53.3%;
  --radius: 0.5rem;
}

.dark {
  --background: 222.2 84% 4.9%;
  --foreground: 210 40% 98%;
  --card: 222.2 84% 4.9%;
  --card-foreground: 210 40% 98%;
  --popover: 222.2 84% 4.9%;
  --popover-foreground: 210 40% 98%;
  --primary: 217.2 91.2% 59.8%;
  --primary-foreground: 222.2 84% 4.9%;
  --secondary: 217.2 32.6% 17.5%;
  --secondary-foreground: 210 40% 98%;
  --muted: 217.2 32.6% 17.5%;
  --muted-foreground: 215 20.2% 65.1%;
  --accent: 217.2 32.6% 17.5%;
  --accent-foreground: 210 40% 98%;
  --destructive: 0 62.8% 30.6%;
  --destructive-foreground: 210 40% 98%;
  --border: 217.2 32.6% 17.5%;
  --input: 217.2 32.6% 17.5%;
  --ring: 224.3 76.3% 94.1%;
}
```

### 3. Add the Babel preset

Update your babel.config.js:

```javascript
module.exports = function (api) {
  api.cache(true);
  return {
    presets: [
      ['babel-preset-expo', {jsxImportSource: 'nativewind'}],
      'nativewind/babel',
    ],
  };
};
```

### 4. Create or modify your metro.config.js

Create a `metro.config.js` file in the root of your project:

```javascript
const {getDefaultConfig} = require('expo/metro-config');
const {withNativeWind} = require('nativewind/metro');

const config = getDefaultConfig(__dirname);

module.exports = withNativeWind(config, {input: './global.css', inlineRem: 16});
```

### 5. Import your CSS file

Import your CSS file in your entry point (index.ts):

```typescript
import './global.css';
```

### 6. Modify your app.json

Switch the bundler to use the Metro bundler:

```json
{
  "expo": {
    "web": {
      "bundler": "metro"
    }
  }
}
```

### 7. TypeScript setup (optional)

If you're using TypeScript, create a `nativewind-env.d.ts` file with:

```typescript
/// <reference types="nativewind/types" />
```

**Caution:** Do not name this file:

- `nativewind.d.ts`
- The same name as a file or folder in the same directory e.g `app.d.ts` when an `/app` folder exists
- The same name as a folder in `node_modules`, e.g `react.d.ts`

## Try it out

Create a simple component to test your Nativewind setup. Note that CSS is imported in `index.ts`, so you don't need to import it in individual components:

```tsx
import {Text, View} from 'react-native';

export default function App() {
  return (
    <View className="flex-1 items-center justify-center bg-white">
      <Text className="text-xl font-bold text-blue-500">
        Welcome to Nativewind!
      </Text>
    </View>
  );
}
```

This example shows:

- Using `className` prop to style components
- Tailwind utility classes like `flex-1`, `items-center`, `justify-center`
- Color utilities like `bg-white`, `text-blue-500`
- Typography utilities like `text-xl`, `font-bold`

If you see the styled text centered on a white background, Nativewind is working correctly!
