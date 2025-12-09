# Color Customization Guide for SalehTZ Blog

## Where to Customize Colors

All color customizations should be made in: `assets/css/_custom.scss`

## How It Works

The LoveIt theme uses SCSS variables for all colors. To customize, simply uncomment the variable you want to change in `_custom.scss` and modify its value.

## Available Color Variables

### 🎨 Background Colors
```scss
$global-background-color: #fff;                    // Light mode background
$global-background-color-dark: #292a2d;            // Dark mode background
```

### 📝 Text Colors
```scss
$global-font-color: #161209;                       // Light mode text
$global-font-color-dark: #a9a9b3;                  // Dark mode text
$global-font-secondary-color: #a9a9b3;             // Light mode secondary text
$global-font-secondary-color-dark: #5d5d5f;        // Dark mode secondary text
```

### 🔗 Link Colors
```scss
$global-link-color: #161209;                       // Light mode links
$global-link-color-dark: #a9a9b3;                  // Dark mode links
$global-link-hover-color: #2d96bd;                 // Light mode link hover (blue)
$global-link-hover-color-dark: #fff;               // Dark mode link hover

// Content area links (inside posts)
$single-link-color: #2d96bd;                       // Light mode content links
$single-link-color-dark: #55bde2;                  // Dark mode content links
$single-link-hover-color: #ef3982;                 // Light mode content link hover (pink)
$single-link-hover-color-dark: #bdebfc;            // Dark mode content link hover
```

### 🎯 Header Colors
```scss
$header-background-color: #f8f8f8;                 // Light mode header background
$header-background-color-dark: #252627;            // Dark mode header background
$header-hover-color: #161209;                      // Light mode header hover
$header-hover-color-dark: #fff;                    // Dark mode header hover
```

### 💻 Code Block Colors
```scss
$code-color: #E74C3C;                              // Light mode inline code (red)
$code-color-dark: #E5BF78;                         // Dark mode inline code (gold)
$code-background-color: #f5f5f5;                   // Light mode code background
$code-background-color-dark: #272C34;              // Dark mode code background
```

### 📦 Other Element Colors
```scss
$blockquote-color: #6bd6fd;                        // Light mode blockquote (cyan)
$blockquote-color-dark: #59c5ec;                   // Dark mode blockquote
$global-border-color: #f0f0f0;                     // Light mode borders
$global-border-color-dark: #363636;                // Dark mode borders
$table-background-color: #fff;                     // Light mode table background
$table-background-color-dark: #272c34;             // Dark mode table background
```

## Example Customizations

### Example 1: Purple Theme (Already Applied)
```scss
$custom-light-purple: #8B5DFF;

[theme=dark] h1 {
    color: $custom-light-purple;
}

[theme=dark] h2,
[theme=dark] h3 {
    color: rgba($custom-light-purple, 0.75);
}
```

### Example 2: Change Link Colors to Green
```scss
// Add this to _custom.scss
$global-link-hover-color: #2ecc71;                 // Green hover for light mode
$single-link-color: #27ae60;                       // Green links in content
$single-link-hover-color: #2ecc71;                 // Brighter green on hover
```

### Example 3: Dark Blue Background Theme
```scss
$global-background-color-dark: #1a1d23;            // Darker blue-ish background
$header-background-color-dark: #151820;            // Even darker header
$code-background-color-dark: #1e2228;              // Matching code blocks
```

### Example 4: Warm Color Scheme
```scss
$global-link-hover-color: #ff6b6b;                 // Coral red
$single-link-color: #ee5a6f;                       // Warm pink
$code-color: #ff7f50;                              // Coral orange for inline code
$blockquote-color: #ffa07a;                        // Light salmon for quotes
```

## Testing Your Changes

1. Make changes in `assets/css/_custom.scss`
2. Hugo will auto-reload if you're running `hugo server -D`
3. Refresh your browser to see the changes
4. Toggle between light/dark modes to test both themes

## Tips

- Always define both light and dark mode colors for consistency
- Use hex colors (#000000) or rgba colors for transparency
- Test your colors for accessibility (good contrast ratios)
- Keep a backup of colors you like before experimenting

## Color Tools

- [Coolors.co](https://coolors.co/) - Generate color palettes
- [Adobe Color](https://color.adobe.com/) - Color wheel and harmony rules
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) - Check accessibility
