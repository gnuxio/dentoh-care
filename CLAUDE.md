# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

DentohCare is a dental clinic website built with Astro and Tailwind CSS v4. The site is a single-page application (SPA) with smooth scrolling sections showcasing dental services, testimonials, and contact information. The content is in Spanish and targets dental care patients.

## Development Commands

```bash
# Install dependencies
npm install

# Start development server (runs at localhost:4321)
npm run dev

# Build for production (outputs to ./dist/)
npm run build

# Preview production build locally
npm run preview

# Run Astro CLI commands
npm run astro -- <command>
```

## Architecture

### Tech Stack
- **Framework**: Astro 5.x (Static Site Generator)
- **Styling**: Tailwind CSS v4 (configured via Vite plugin)
- **Language**: TypeScript (minimal usage, mostly .astro files)
- **Content Language**: Spanish

### Project Structure

```
src/
├── components/          # Astro components for each section
│   ├── HeroSection.astro
│   ├── AboutSection.astro
│   ├── ServicesSection.astro
│   ├── TestimonialsSection.astro
│   ├── ContactSection.astro
│   └── Footer.astro
├── layouts/
│   └── Layout.astro     # Base HTML layout with global styles
├── pages/
│   └── index.astro      # Main page composing all sections
└── styles/
    └── global.css       # Tailwind CSS import
```

### Component Architecture

**Single Page Layout**: The main `index.astro` file composes all sections into a single scrollable page with:
- Fixed navigation bar with scroll-based transparency effect
- Anchor-linked sections for smooth navigation (#inicio, #sobre-mi, #servicios, #testimonios, #contacto)
- Client-side JavaScript for navbar background transitions on scroll

**Component Pattern**: Each section is a self-contained `.astro` component with:
- Frontmatter for data definitions (arrays of services, testimonials, etc.)
- Inline SVG icons rendered via helper functions
- Scoped `<style>` blocks for component-specific animations
- `<script>` blocks for scroll-triggered animations using IntersectionObserver

**Styling Approach**:
- Tailwind utility classes for layout and styling
- Custom CSS animations defined in `<style>` blocks (fade-up, fade-in, slide-left, slide-right)
- Gradient backgrounds and modern design patterns (blur effects, rounded corners, shadows)
- Scroll-triggered animations using `.fade-on-scroll` class pattern with IntersectionObserver

### Key Patterns

1. **Animation System**: Components use IntersectionObserver to trigger fade-in animations when scrolling into view. Pattern:
   ```javascript
   const observer = new IntersectionObserver((entries) => {
     entries.forEach((entry) => {
       if (entry.isIntersecting) entry.target.classList.add("fade-in-visible");
     });
   }, { threshold: 0.2 });
   ```

2. **Icon Rendering**: SVG icons are defined as functions in component frontmatter and injected using `set:html`:
   ```astro
   <div set:html={iconSvg(service.icon)}></div>
   ```

3. **Data-Driven Components**: Services, testimonials, and other content are defined as arrays in frontmatter and mapped in the template.

## Tailwind Configuration

Tailwind CSS v4 is integrated via the Vite plugin (`@tailwindcss/vite`) configured in `astro.config.mjs`. No separate `tailwind.config.js` file exists - using default Tailwind configuration.

Global styles are imported in `src/styles/global.css` with a single line: `@import "tailwindcss";`

## Important Notes

- **No Build Step for Styles**: Tailwind is processed through Vite during the Astro build process
- **Spanish Content**: All user-facing text is in Spanish - maintain this when adding content
- **No Routing**: This is a single-page site - all navigation is anchor-based within index.astro
- **Images**: External images from Unsplash are used (e.g., in HeroSection.astro)
- **No API/Backend**: This is a static site with no server-side functionality
