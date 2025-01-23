# Testing React Components 1

## Tools

- [Vitest](https://vitest.dev/guide/) is a Vite-native testing framework and compatible with [Jest](https://jestjs.io/).
- [React Testing Library](https://github.com/testing-library/react-testing-library/blob/main/README.md) provides React DOM testing utilities.
- [jest-dom](https://github.com/testing-library/jest-dom/blob/main/README.md) is used for DOM assertions.
- [jsdom](https://github.com/jsdom/jsdom/blob/main/README.md) is used for simulating a browser environment in Node for testing purposes.

## Setup testing environment

1. Install libraries: `npm install --save-dev vitest jsdom @testing-library/react @testing-library/jest-dom @types/jest`
1. Add test script to `package.json`:

    ```json
    "test": "vitest run"
    ```

1. Create `vitest-setup.js`:

    ```js
    import {afterEach} from 'vitest';
    import {cleanup} from '@testing-library/react';
    import '@testing-library/jest-dom/vitest';

    // reset jsdom (simulated browser) after each test
    afterEach(() => {
      cleanup();
    });
    ```

1. Update `vite.config.ts` by addin test configuration:

    ```ts
    /// <reference types="vitest/config" /> // https://vitest.dev/config/
    import {defineConfig} from 'vite';
    import react from '@vitejs/plugin-react-swc';

    // https://vite.dev/config/
    export default defineConfig({
      plugins: [react()],
      test: {
        environment: 'jsdom',
        globals: true,
        setupFiles: './vitest-setup.js',
      },
    });
    ```

## Simple examples

### Profile view component

1. Create `src/tests/Profile.test.tsx`:

    ```ts
    import {render, screen} from '@testing-library/react';
    import Profile from '../views/Profile';

    test('renders correct content for the headline', () => {

      // render the Profile component in jsdom (simulated browser)
      render(<Profile />);

      // find the element with the text 'Profile'
      const element = screen.getByText(
        'Profile',
      );
      
      // check that the element is found (not undefined)
      expect(element).toBeDefined();
    });
    ```

1. Run tests: `npm test`
   - Finds and runs all files with extensions `.test.ts(x)` and `.spec.ts(x)`
   - Use `tsx` extension for React component tests and `ts` for "plain" TypeScript files (e.g. unit tests).
1. Change the content of `h2` and try again

### Dummy upload component

_Upload.tsx_:

```tsx
import {useState} from 'react';

const Upload = () => {
  const [uploading, setUploading] = useState(false);
  return (
    <>
      <h2>Upload</h2>
      <button
        onClick={() => {
          setUploading(true);
          setTimeout(() => {
            setUploading(false);
          }, 3000);
        }}
      >
        Upload
      </button>
      {uploading && <p>Uploading...</p>}
    </>
  );
};

export default Upload;
```

_Upload.test.tsx_:

```tsx
import {fireEvent, render, screen} from '@testing-library/react';
import Upload from '../views/Upload';

test('renders h2 headline', () => {
  render(<Upload />);
  const header = screen.getByRole('heading', {
      level: 2,
    })
  expect(header).toBeDefined();
});

test('displays uploading notification after button is clicked', () => {
  render(<Upload />);
  // simulates clicking the button
  fireEvent.click(screen.getByRole('button'));
  expect(screen.getByText('Uploading...')).toBeDefined();
});
```

## Assignment

1. Create a git branch `component-test`.
1. Setup the testing environment
1. Try out the examples
1. In the following weeks: Write some tests for your own components (individual and group project).
