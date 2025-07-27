# Tailwind CSS v4.0 Project

## Overview
This project is a modern web application styled with **Tailwind CSS v4.0**, a utility-first CSS framework that leverages modern CSS features like OKLCH colors, cascade layers, and CSS-first configuration. It includes a sample HTML/CSS setup and an `llms.txt` file for AI-assisted development, providing structured documentation for large language models (LLMs) to understand Tailwind's classes and configuration.

## Features
- **Tailwind CSS v4.0**: Fast, lightweight styling with utilities for layout, Flexbox, Grid, typography, and more.
- **CSS-First Configuration**: Custom theme defined via `@theme` directive in CSS.
- **AI Support**: `llms.txt` file for seamless integration with AI coding tools.
- **Responsive Design**: Uses Tailwind's breakpoint-driven utilities for mobile-first layouts.
- **Modern Browser Support**: Optimized for Safari 16.4+, Chrome 111+, Firefox 128+.

## Prerequisites
- **Node.js** and **npm** (v16 or later recommended).
- Modern web browser (e.g., Chrome, Firefox, Safari).
- Optional: Vite for development and build.

## Installation
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-repo/tailwind-project.git
   cd tailwind-project
   ```

2. **Install Dependencies**:
   ```bash
   npm install tailwindcss @tailwindcss/cli
   ```

3. **Set Up Tailwind CSS**:
   Create an `input.css` file:
   ```css
   @import "tailwindcss";
   @theme {
     --font-sans: "Inter", system-ui, sans-serif;
     --color-brand-500: oklch(0.637 0.237 25.331);
     --breakpoint-md: 48rem;
     --spacing: 0.25rem;
   }
   @source "./src/**/*.{html,js,jsx,ts,tsx}";
   ```

4. **Build CSS**:
   ```bash
   npx @tailwindcss/cli -i input.css -o output.css
   ```

5. **Include in HTML**:
   ```html
   <link href="./output.css" rel="stylesheet">
   ```

6. **Optional: Use Vite**:
   Install Vite plugin:
   ```bash
   npm install @tailwindcss/vite
   ```
   Configure `vite.config.js`:
   ```javascript
   import tailwindcss from '@tailwindcss/vite'
   export default { plugins: [tailwindcss()] }
   ```

## Usage
- **Development**:
  Run the development server with Vite:
  ```bash
  npx vite
  ```
  Or build manually:
  ```bash
  npx @tailwindcss/cli -i input.css -o output.css --watch
  ```

- **Example HTML**:
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="./output.css" rel="stylesheet">
    <title>Tailwind v4.0 Project</title>
  </head>
  <body class="font-sans bg-brand-500 flex flex-col items-center p-4">
    <h1 class="text-3xl font-bold text-white text-shadow-md md:text-4xl">Welcome</h1>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
      <div class="bg-white p-4 rounded-lg">Item 1</div>
      <div class="bg-white p-4 rounded-lg col-span-2">Item 2</div>
    </div>
    <button class="px-4 py-2 border-2 border-white rounded-md">Click Me</button>
  </body>
  </html>
  ```

- **AI Integration**:
  The `llms.txt` file provides structured Tailwind CSS v4.0 documentation for AI tools. Host it at `your-site.com/llms.txt` or feed it directly to your LLM for enhanced code completion and styling suggestions. It covers:
  - Base styles
  - Layout utilities (e.g., `flex`, `grid`, `container`)
  - Flexbox and Grid utilities
  - Typography, backgrounds, borders, and v4.1 features (e.g., `text-shadow-*`, `mask-*`)

## Project Structure
```
tailwind-project/
├── src/
│   ├── input.css         # Tailwind CSS configuration
│   ├── output.css       # Compiled CSS
│   ├── index.html       # Main HTML file
├── llms.txt            # AI-compatible Tailwind docs
├── vite.config.js      # Vite configuration (optional)
├── package.json        # Node.js dependencies
└── README.md           # This file
```

## Customization
- **Extend Theme**:
  Add custom theme values in `input.css`:
  ```css
  @theme {
    --color-lagoon: oklch(0.72 0.11 221.19);
    --font-body: "Roboto", sans-serif;
  }
  ```
  Use in HTML: `<div class="bg-lagoon font-body">`

- **Custom Utilities**:
  ```css
  @utility tab-* { tab-size: --value(--tab-size-*); }
  @theme { --tab-size-4: 4; }
  ```
  Use: `<pre class="tab-4">`

## Contributing
1. Fork the repository.
2. Create a feature branch: `git checkout -b feature-name`
3. Commit changes: `git commit -m "Add feature"`
4. Push to branch: `git push origin feature-name`
5. Open a pull request.

## Resources
- [Tailwind CSS v4.0 Docs](https://tailwindcss.com/docs)
- [Theme Variables](https://tailwindcss.com/docs/theme-variables)
- [Browser Support](https://tailwindcss.com/docs/browser-support)
- [Upgrade Guide](https://tailwindcss.com/docs/upgrade-guide)

## License
MIT License. See [LICENSE](LICENSE) for details.
