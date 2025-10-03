# Building Canva Apps with Canva Dev MCP

## Overview

Canva Dev MCP can significantly accelerate Canva app development by generating boilerplate code, implementing features, and providing real-time guidance. This guide explains how to leverage LLMs effectively with the Canva Apps SDK to build React-based applications for the Canva platform.

## Prerequisites

Before starting, ensure you have:
- Node.js 18+ and npm 9+ installed
- A Canva account with developer access
- Basic understanding of React and TypeScript
- Access to an LLM (ChatGPT, Claude, etc.)

## Getting Started

### 1. Initial Setup

First, create a new Canva app using the official CLI. The LLM can help generate the initial commands:

```bash
npm install -g @canva/create-app
create-canva-app my-awesome-app
cd my-awesome-app
npm install
```

For detailed setup instructions, refer to the [Quick Start guide](https://www.canva.dev/docs/apps/quickstart/).

### 2. Project Structure

Ask your LLM to explain the generated project structure:

```
my-awesome-app/
├── src/
│   ├── app.tsx          # Main app component
│   ├── index.tsx        # Entry point
│   └── styles.css       # App styles
├── canva.config.js      # App configuration
├── package.json         # Dependencies
└── tsconfig.json        # TypeScript config
```

## Creating Code with LLM Assistance

### Effective Prompting Strategies

When working with an LLM to generate Canva app code, provide context about:

1. **SDK Version**: Specify you're using the latest Canva Apps SDK
2. **App Type**: Clarify if building a content, publish, or editing app
3. **Features**: List specific Canva APIs you need to use

#### Example Prompt:

```
"Create a React component for a Canva app that:
- Uses the Canva Apps SDK design import API
- Allows users to upload images from URLs
- Implements proper error handling
- Uses TypeScript with proper type definitions"
```

### Core App Development

The LLM can generate the main app component structure. Here's a typical pattern:

```typescript
import React, { useState } from "react";
import { Button, Rows, Text, Title } from "@canva/app-ui-kit";
import { addNativeElement } from "@canva/design";
import styles from "styles/app.css";

export const App = () => {
  const [isLoading, setIsLoading] = useState(false);

  const handleAddElement = async () => {
    setIsLoading(true);
    try {
      await addNativeElement({
        type: "TEXT",
        children: ["Hello from Canva App!"],
      });
    } catch (error) {
      console.error("Failed to add element:", error);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className={styles.scrollContainer}>
      <Rows spacing="2u">
        <Title>My Canva App</Title>
        <Text>Add custom content to your designs</Text>
        <Button 
          variant="primary" 
          onClick={handleAddElement}
          loading={isLoading}
        >
          Add Element
        </Button>
      </Rows>
    </div>
  );
};
```

### Implementing Canva APIs

Request LLM assistance for specific API implementations. Reference the [API documentation](https://www.canva.dev/docs/apps/api/) for available capabilities:

#### Design Interaction APIs
- **Adding Elements**: Use [`addNativeElement`](https://www.canva.dev/docs/apps/api/design-interaction/) for text, shapes, and images
- **Asset Upload**: Implement [`upload`](https://www.canva.dev/docs/apps/api/asset-upload/) for external media
- **User Authentication**: Set up [`auth`](https://www.canva.dev/docs/apps/api/authentication/) for third-party services

#### Example: Image Import Feature

Ask your LLM to generate an image import component:

```typescript
import { upload } from "@canva/asset";
import { addNativeElement } from "@canva/design";

const uploadAndAddImage = async (url: string) => {
  // Upload external image to Canva
  const result = await upload({
    type: "IMAGE",
    url: url,
    mimeType: "image/jpeg",
    thumbnailUrl: url,
  });

  // Add uploaded image to design
  await addNativeElement({
    type: "IMAGE",
    ref: result.ref,
  });
};
```

## Compiling and Building React Apps

### Development Mode

For local development with hot reload:

```bash
npm run start
```

This starts the development server at `http://localhost:8080`. The app automatically recompiles when you save changes.

### Production Build

To compile for production deployment:

```bash
npm run build
```

This command:
1. Transpiles TypeScript to JavaScript
2. Bundles all dependencies
3. Optimizes React for production
4. Generates minified output in `/dist`

### Configuration Management

The `canva.config.js` file controls build settings. Have your LLM help customize:

```javascript
module.exports = {
  appId: "YOUR_APP_ID",
  type: "content",
  features: ["import", "export"],
  apiVersion: "1.0.0",
};
```

## UI Kit Integration

Leverage the [Canva App UI Kit](https://www.canva.dev/docs/apps/app-ui-kit/) for consistent design:

```typescript
import {
  Button,
  FormField,
  MultilineInput,
  Rows,
  Select,
  Switch,
  Title,
} from "@canva/app-ui-kit";
```

Ask your LLM to generate form components using these UI elements for better integration with Canva's design system.

## Testing and Debugging

### Local Testing

1. Run `npm run start`
2. Open Canva and navigate to Apps
3. Click "Create an app" → "Start with an example"
4. Select "Development" and enter your local URL

### LLM-Assisted Debugging

When encountering errors, provide the LLM with:
- Error messages
- Relevant code snippets
- SDK version information
- Browser console output

## Best Practices for LLM Development

1. **Iterative Development**: Build features incrementally, testing each addition
2. **Type Safety**: Always request TypeScript implementations for better IDE support
3. **Error Handling**: Ask for comprehensive error handling in generated code
4. **Documentation**: Request inline comments for complex logic
5. **SDK Compliance**: Verify generated code against [official examples](https://www.canva.dev/docs/apps/examples/)

## Deployment

Once your app is ready:

1. Build the production version: `npm run build`
2. Upload to Canva via the [Developer Portal](https://www.canva.dev/docs/apps/publish/)
3. Submit for review following the [guidelines](https://www.canva.dev/docs/apps/review-guidelines/)

## Resources

- [Canva Apps SDK Documentation](https://www.canva.dev/docs/apps/)
- [API Reference](https://www.canva.dev/docs/apps/api/)
- [Code Examples](https://www.canva.dev/docs/apps/examples/)
- [Developer Portal](https://www.canva.com/developers/)
- [Community Forum](https://community.canva.dev/)

## Conclusion

Canva Dev MCP excel at accelerating Canva app development by generating boilerplate code, implementing features, and solving technical challenges. Combine LLM assistance with official documentation and testing to build robust, user-friendly Canva applications efficiently.