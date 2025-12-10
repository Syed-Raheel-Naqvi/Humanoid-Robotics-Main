# Quickstart Guide: Educational Book Platform

## Prerequisites
- Node.js 18+ 
- npm or yarn package manager
- Git for version control

## Getting Started

### 1. Clone the repository
```bash
git clone <repository-url>
cd <repository-name>
```

### 2. Install dependencies
```bash
npm install
# or
yarn install
```

### 3. Start the development server
```bash
npm run start
# or
yarn start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### 4. Build for production
```bash
npm run build
# or
yarn build
```

The build command creates an optimized, production-ready version of your site in the `build` directory.

## Project Structure

### Important Directories
- `/docs` - Contains educational book content in MDX format
- `/src` - Custom React components and site customization
- `/src/components` - Reusable React components for educational features
- `/static` - Static assets like images, downloadable files
- `/docusaurus.config.js` - Main Docusaurus configuration file
- `/sidebars.js` - Navigation sidebar configuration

### Adding New Content
To add a new chapter or page:
1. Create an MDX file in the `/docs` directory
2. Add an entry to the `sidebars.js` file to make it appear in the navigation
3. Use MDX syntax to include interactive elements, code examples, and other components

## Key Features

### MDX Content
- Full React component support within Markdown
- Code syntax highlighting with copy buttons
- Interactive examples and demos
- Mathematical expressions using LaTeX

### Navigation
- Auto-generated table of contents
- Previous/Next chapter navigation
- Search functionality across all content
- Breadcrumb navigation

### Custom Components
- Code playgrounds for interactive examples
- Quiz components for self-assessment
- Expandable sections for detailed explanations
- Image carousels for visual content

## Configuration

### Site Metadata
Edit `docusaurus.config.js` to change:
- Site title and tagline
- Favicon
- Social media metadata
- Analytics tracking
- Search configuration

### Navigation
Update `sidebars.js` to:
- Organize chapters and sections
- Create nested navigation structures
- Control which pages appear in navigation

## Deployment

The site can be deployed to various hosting platforms:

### GitHub Pages
```bash
npm run deploy
```

### Vercel, Netlify, or other static hosting
Simply deploy the `build` directory to your preferred hosting platform.

## Further Reading

- [Docusaurus Documentation](https://docusaurus.io/docs)
- [MDX Documentation](https://mdxjs.com/)
- [React Documentation](https://reactjs.org/)