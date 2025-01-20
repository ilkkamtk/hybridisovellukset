# Testing React Components

[Vitest](https://vitest.dev/guide/) is a Vite-native testing framework and compatible with [Jest](https://jestjs.io/).

## Setup testing environment

1. Install libraries for testing: `npm install --save-dev vitest jsdom @testing-library/react @testing-library/jest-dom @types/jest`

1. Add test script to `package.json`:

    ```json
    "test": "vitest run"
    ```

1. Create `vitest-setup.js`:

    ```js
    import {afterEach} from 'vitest';
    import {cleanup} from '@testing-library/react';
    import '@testing-library/jest-dom/vitest';

    afterEach(() => {
      cleanup();
    });
    ```

1. Update `vite.config.ts`:

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

## Testing the first component

1. Create `src/tests/Home.test.tsx`:

    ```ts
    import {render, screen} from '@testing-library/react';
    import Home from '../components/Home';

    test('renders correct content for the headline', () => {

      render(<Home />);

      const element = screen.getByText(
        'My Media',
      );
      expect(element).toBeDefined();
    });
    ```

1. Test by running `npm run test`
1. Change the content of `h2` and try again
