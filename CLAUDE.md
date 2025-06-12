# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static HTML website for M1C2, a product studio founded by Zak Hap. The site showcases studio tenets, projects in various stages of development, and blog posts about art, technology, and product development.

## Architecture

- **Static HTML Site**: No build process or dependencies required
- **Inline CSS**: Each HTML file contains its own embedded styles in `<style>` tags
- **Shared Design System**: Consistent dark theme with monospace font across all pages
  - Background: `#2d2d2d`
  - Text: `#f0f0f0` 
  - Accent: `#888`
  - Links maintain underline styling with hover states
- **Navigation**: Simple link-based navigation between pages
- **Interactive Elements**: Minimal JavaScript for hover effects (e.g., Shō image on about.html)

## Content Structure

- `index.html` - Main landing page with tenets, current projects, and blog links
- `about.html` - Personal bio for Zak Hap with interactive elements
- `projects.html` - Comprehensive project pipeline (In Development, In Design, In Ideation)
- Blog posts: `latent-space-as-medium.html`, `signatures.html`, `tyranny-of-pre-digestion.html`, `prompters.html`, `agent-response.html`
- `images/` - Static assets (e.g., sho1.png)

## Development Notes

- No package.json or build tools - direct file editing
- CSS is embedded in each HTML file for self-contained pages
- Maintain consistent styling when adding new pages
- Studio philosophy emphasizes "test in production" and iterative development
- External links point to live demos on Vercel/Netlify for active projects