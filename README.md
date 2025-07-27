# Tailwind CSS v4 LLM Prompt

A comprehensive prompt file for training and guiding LLMs (Large Language Models) to work effectively with Tailwind CSS v4.

## What's Included

This prompt provides LLMs with complete knowledge of:

- **CSS-first configuration** using `@theme` directive
- **Complete utility class reference** with all v4 classes
- **New v4 features** like container queries, 3D transforms, text shadows, masks
- **Breaking changes** from v3 and how to avoid deprecated patterns
- **Modern best practices** for performance and development
- **Production-ready component examples**
- **Migration guidance** from v3 to v4
- **Framework integration** (Svelte 5 example included)

## Key v4 Changes Covered

- CSS-first configuration instead of JavaScript config files
- New Oxide engine performance improvements
- OKLCH color system for wider gamut
- Container queries built into core
- Enhanced gradients, 3D transforms, and text shadows
- Modern browser requirements (Safari 16.4+, Chrome 111+, Firefox 128+)

## Usage

1. Copy the content from `tailwind-llms.txt`
2. Use it as a system prompt or training material for your LLM
3. The LLM will provide accurate v4 code and avoid deprecated v3 patterns

## Features for LLMs

✅ **Complete class reference** - Every utility class available in v4  
✅ **Error prevention** - Knows what deprecated patterns to avoid  
✅ **Modern patterns** - Uses latest v4 features and best practices  
✅ **Framework agnostic** - Works with any framework using Tailwind  
✅ **Performance optimized** - Leverages v4's new capabilities  

## Example Output

With this prompt, LLMs will generate modern v4 code like:

```html
<!-- Container queries for true responsive design -->
<div class="@container">
  <div class="p-4 @lg:p-8 @xl:p-12">
    Responds to container, not viewport
  </div>
</div>

<!-- 3D transforms -->
<button class="
  perspective-1000 transform-3d
  hover:rotate-x-12 hover:scale-105
  transition-all duration-200
">
  3D Button
</button>

<!-- Modern CSS-first configuration -->
@import "tailwindcss";
@theme {
  --color-brand: oklch(0.5 0.2 240);
  --font-display: "Inter", sans-serif;
}
```

## Contributing

Feel free to submit issues or pull requests to improve the prompt coverage or accuracy.

## License

MIT License - Use freely for training LLMs or development guidance.
