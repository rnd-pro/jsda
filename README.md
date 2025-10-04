# JSDA - JavaScript Distributed Assets

JavaScript Distributed Assets (JSDA) represents a modern evolution of the [JAMStack](https://jamstack.org/) philosophy, extending beyond static site generation to encompass comprehensive web development patterns. JSDA addresses complex use cases including hybrid web applications, dynamic server-side rendering, advanced dependency management, and optimized micro-frontend architectures.

JSDA embraces leveraging native web technologies to their full potential without unnecessary complexity. Rather than reinventing existing solutions, JSDA provides workflow principles for utilizing web standards with maximum flexibility and minimal overhead.

> JSDA is a set of principles, conventions and recommendations that doesn't force you to use specific tools.

### The Central Concept

JSDA transforms standard JavaScript ESM modules into text-based web asset generation endpoints. This approach mirrors PHP's model but leverages modern JavaScript's universal capabilities.

**Key Principle**: A JSDA file is a standard JavaScript module that exports a string representing web assets like HTML, CSS, SVG, Markdown, or JSON data.

### File Naming Convention

Asset types are determined through intuitive file naming conventions:
- **index.html.js** → HTML generation
- **styles.css.js** → CSS generation
- **icon.svg.js** → SVG generation
- **content.md.js** → Markdown generation

Static asset generation endpoints should be named as **index.*.js**. The output file structure reflects the source:
```
src/
├── index.html.js          → dist/index.html
├── styles/
│   └── index.css.js       → dist/styles/index.css
└── assets/
    └── logo/
        └── index.svg.js   → dist/assets/logo/index.svg
```

### Basic Example

```js
// index.html.js

// Import templating utilities (optional)
import { applyData, html } from 'jsda-kit';

// Define reusable template
const template = html`
<div class="user-profile">
  <div class="user-name">{{firstName}} {{lastName}}</div>
  <div class="user-role">{{role}}</div>
</div>
`;

// Fetch data asynchronously (optional)
const userData = await (await fetch('./data/user-data.json')).json();

// Export the rendered asset
export default applyData(template, userData);
```

**Generated Output** (`index.html`):
```html
<div class="user-profile">
  <div class="user-name">John Doe</div>
  <div class="user-role">Senior Developer</div>
</div>
```

The result can be saved to the file system as a static asset or served dynamically. This provides templates, module/component structure, asynchronous data fetching, and caching out of the box—most features are provided by the platform itself without library bloat and with minimal project configuration.

## Reasonable Minimalism

Modern AI tools are driving a huge development paradigm shift. While AI assistants significantly boost productivity, new challenges emerge: we need to optimize token consumption industry-wide and drop obsolete legacy patterns. Otherwise, AI will reproduce inefficient solutions that appeared during early platform development.

Many popular development tools were created to address missing native platform capabilities. They became foundations for additional abstraction layers, resulting in solutions that duplicate platform-native features available today.

Consider CSS pre- and post-processors: they introduce custom syntax and additional processing tools. But do we really need them? Modern CSS variables provide more powerful dynamic runtime styling, and JavaScript enables custom logic and automation for building stylesheets on both server and client sides.

Similar examples include using additional tools for micro-frontend architectures instead of standard **importmap**, or custom component models instead of the Custom Elements standard.

We don't need to duplicate existing capabilities. Native platform features are more reliable, resource-efficient, well-documented, and more flexible than external black boxes. JSDA principles rely on this minimalistic yet powerful approach.

## JavaScript as the New PHP

The JSDA approach shares similarities with PHP (Hypertext Preprocessor). Why choose JSDA over established PHP? The answer lies in Occam's razor and the DRY principle (Don't Repeat Yourself). JavaScript is simply more universal — it works on both server and browser.

The ability to use identical code in Node.js and browsers is crucial, as is sharing project settings, types, and development environment tools. This creates significant economies in testing, documentation, maintenance, and more.

## Package Management

JSDA leverages standard package managers (like npm, pnpm, or yarn) to treat web assets as versionable, distributable dependencies. Since every JSDA asset is fundamentally a JavaScript module, you can publish, install, and import them just like any other package.

This powerful pattern allows you to:
- **Share & Reuse**: Distribute UI components, style libraries, SVG icons, or even entire page layouts across multiple projects.
- **Version Control**: Manage your assets with semantic versioning, ensuring stable and predictable updates.
- **Simplify Dependencies**: Use the tools you already know. No special registries or plugins are required.

For example, after installing a shared component package (`npm install @my-org/ui-card`), you can import and use it directly:

```js
// index.html.js
import card from '@my-org/ui-card/index.html.js'; // Imports the card's HTML string

export default `<h1>My Page</h1> ${card}`;
```

## Easy Two-Way Conversion

JSDA enables a seamless two-way workflow between raw assets and JavaScript modules. While JSDA modules generate text assets, existing assets like SVGs or Markdown files can be easily wrapped in a JSDA module for processing or inclusion in your project.

For instance, you can read an SVG file created in a visual editor, process it, and export it as a JSDA:
```js
// my-logo.svg.js
import fs from 'fs';
import { mySvgProcessor } from './my-svg-processor.js';

export default mySvgProcessor(fs.readFileSync('./src/svg/my-logo.svg'));
```

## Distributed Composition

This concept is particularly promising. We build solutions from modules that can collect components from different endpoints. Some are simple JavaScript files, others are retrieved from external HTTPS endpoints (including CDNs). This leverages the native ESM standard, which includes built-in security and caching policies.

Imagine requesting application parts as complete external services that provide everything needed for operation. This enables new developer interaction models where small teams provide services independently, using smart-contract consumption-based payroll systems. It also supports Open Source project financing. We know about microservices and micro-frontends — now it's time for micro-products.

Two basic resource provision strategies exist:
1. Serve JS files as-is, building all dependencies at runtime
2. Use middleware build & cache stages where HTTPS endpoints serve results for specific entry points

The second option is more relevant for micro-product providers, similar to how JavaScript CDNs work (e.g., esm.run, JSPM.io, skypack.dev).

> JSDA's distributed composition model represents a paradigm shift toward granular, service-oriented web development. This architecture enables applications to dynamically compose functionality from multiple sources while maintaining security and performance.

## Content Delivery Networks (CDN)

Once JSDA has rendered its content, the generated assets can be deployed to Content Delivery Networks (CDNs) for enhanced performance, global distribution, and cross-project asset sharing. Following JAMStack principles, JSDA leverages CDNs to achieve maximum reliability, speed, and scalability.

**Developer Experience**
- **Version Management**: CDNs support versioned asset delivery with cache-busting strategies
- **Hot-Swapping**: Update assets without server restarts or application downtime
- **Analytics & Monitoring**: Built-in performance metrics and usage analytics

### CDN Deployment Strategies

**Static Asset Distribution**

Generated assets ready for CDN deployment:
```
dist/
├── index.html             → https://cdn.example.com/v1.2.0/index.html
├── styles/
│   └── index.css          → https://cdn.example.com/v1.2.0/styles/index.css
└── components/
    └── user-profile.js    → https://cdn.example.com/v1.2.0/components/user-profile.js
```

### Security and Reliability

**Content Integrity**
- **Subresource Integrity (SRI)**: Verify asset integrity using cryptographic hashes
- **HTTPS Enforcement**: All CDN endpoints use secure connections by default
- **Access Control**: Configure CORS policies and access restrictions

By leveraging CDNs effectively, JSDA applications achieve enterprise-grade performance and reliability while maintaining the flexibility and simplicity that define the JSDA philosophy.

## TypeScript Usage Convention

TypeScript - is an incredibly significant part of the modern JavaScript development ecosystem. But we have a few caveats about using it.

JSDA conceptually relies on the ESM standard and its composition model. Each module should be a potential entry point for middleware-cached results, even as part of a larger application. Therefore, it's important to skip additional build stages and interoperability overhead between different ecosystems.

We use a well-adopted approach that combines TypeScript static analysis benefits with vanilla JavaScript flexibility: JSDoc-based type definitions. This approach supports additional definition files (*.d.ts), utilities, global and complex definitions. Moreover, JSDoc enables documentation generation and integration with AI-driven workflows.

More information about JSDoc type definitions: https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html

## Code Highlighting Convention
**Pattern:** Prefix comments for IDE support
**Purpose:** Enable syntax highlighting and IntelliSense in string templates

```js
// Enable HTML highlighting
const htmlTemplate = /*html*/ `
<div class="component">
  <h2>{{title}}</h2>
  <p>{{description}}</p>
</div>
`;

// Enable CSS highlighting
const styles = /*css*/ `
.component {
  padding: 1rem;
  border-radius: 0.5rem;
  background: var(--bg-color);
}
`;

// Enable SVG highlighting
const icon = /*svg*/ `
<svg viewBox="0 0 24 24">
  <path d="M12 2L2 7v10c0 5.55 3.84 10 9 11 1.09-.09 2 .73 2 1.84"/>
</svg>
`;
```

## Markdown

Markdown is a lightweight, easy-to-use syntax for styling text. It's excellent for quickly creating web content and has become the de facto standard for LLM-based tool interactions. We use Markdown to describe contexts and represent formatted input/output.

As a text format, Markdown integrates seamlessly with the JSDA ecosystem: building contexts from parts and modules, providing text data for direct web access for the LLM-tools, and more. We use Markdown for web articles and documentation content.

## Hybrid Widgets

JSDA's key advantage is versatility — it works on both server and client sides. Consider a hybrid web application where part of the document renders on the server while other parts require dynamic client control. The simplest way to specify which parts should be hydrated is using the Custom Elements standard.

This enables native lifecycle methods to activate markup and bind data and handlers at specific locations. No need to invent placeholder-based or other additional-data-based workflows (like many other solutions) — we have everything needed. Custom Elements provide access to their inner content via the standard DOM API.

To insert external application parts, simply use the `innerHTML` interface at browser runtime or include pre-rendered HTML parts on your server.

## Learning Curve

JSDA features a gentle learning curve designed for developers of all experience levels. This elegant and flexible architecture requires minimal JavaScript expertise to get started — you only need familiarity with two fundamental concepts: [ESM (ES Modules)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) for modular code organization and [Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) for dynamic string composition.

## Ecosystem

Implementing JSDA requires some additional lightweight tools. The exact set is your choice, but we're developing tools to provide our detailed vision and make getting started more convenient.

**[Symbiote.js](https://rnd-pro.com/symbiote/)** is our frontend library that describes templates and reactive bindings in HTML format using native tag attributes. This enables flexible rendering manipulation and using pre-rendered markup as templates, making Custom Elements Server Side Rendering simple and effective. The library is fast and powerful, providing the same developer experience as other modern frontend libraries while extending (not replacing) the native DOM API.

**[JSDA-Kit](https://github.com/rnd-pro/jsda-kit)** is an isomorphic (primarily Node.js) library providing toolsets for Static Site Generation, Server Side Rendering, and dynamic JSDA real-time servers.

**[Cloud Images Toolkit](https://github.com/rnd-pro/JSDA-server)** offers project media asset collection management, publishing, and collaborative work capabilities.

> Join us in the future of web development!