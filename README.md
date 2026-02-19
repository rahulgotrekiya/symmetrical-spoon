# Blog. Template

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

A beautiful, modern blog template built with pure HTML, CSS, and JavaScript. This template features a clean design with Libre Baskerville typography, smooth animations, and a fully responsive layout.

## 🚀 Live Demo

**[View Live Demo →](https://blog.gotrekiya.in)**

Check out the live version of this template in action!

## Features

✨ **Modern Design**
- Clean, minimalist interface
- Libre Baskerville serif typography
- Smooth animations and transitions
- Fully responsive layout

📄 **Complete Pages**
- Homepage with post feed
- Individual post/article page
- User profile page
- Settings page (with avatar upload)
- Dashboard (manage posts)
- Create post page

🎨 **UI Components**
- Navigation bar with dropdown
- Post cards with hover effects
- Comment system
- Like/favorite buttons
- Forms with validation styles
- Modal dialogs
- Data tables
- Category badges

## Project Structure

```
blog-template/
├── index.html              # Homepage
├── pages/
│   ├── post.html          # Article/post page
│   ├── profile.html       # User profile
│   ├── settings.html      # Account settings
│   ├── dashboard.html     # User's posts dashboard
│   └── create-post.html   # Post creation form
├── css/
│   └── style.css          # All styles
├── js/
│   └── script.js          # JavaScript functions
├── images/
│   ├── avatars/           # User avatars
│   ├── posts/             # Post cover images
│   └── sample-post-cover.png
└── README.md
```

## Quick Start

1. **Download the template**
   ```bash
   cd ~/Documents/blog-template
   ```

2. **Open in browser**
   - Simply open `index.html` in your web browser
   - Or use a local server (recommended):
   ```bash
   # Python 3
   python3 -m http.server 8000
   
   # Node.js
   npx serve
   ```

3. **Start customizing**
   - Edit HTML files to change content
   - Modify `css/style.css` to customize design
   - Update `js/script.js` for functionality

## Customization

### Colors

The color scheme is defined using CSS variables in `css/style.css`:

```css
:root {
    --primary: #000000;
    --primary-hover: #333333;
    --accent: #6366f1;
    /* ... more colors */
}
```

### Typography

The template uses **Libre Baskerville** for headings and **Inter** for body text. To change fonts:

1. Update the `@import` statement in `css/style.css`
2. Modify the `--font-serif` and `--font-sans` variables

### Layout

- **Container width**: Adjust `.container` max-width in CSS
- **Spacing**: Modify the spacing utility classes
- **Grid layouts**: Update `.post-feed` and `.settings-grid` properties

## Sample Content

The template includes sample content to demonstrate all features:

- **3 sample posts** on the homepage
- **1 complete article** with full content and comments
- **User profile** with avatar and bio
- **Settings page** with all form fields
- **Sample images** in the images directory

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Features in Detail

### Homepage
- Hero section with call-to-action
- Post feed grid with cards
- Category filters
- Responsive navigation

### Post Page
- Full article content with typography styles
- Author information
- Like and share buttons
- Comments section
- Related posts (to be implemented)

### Profile Page
- User avatar and bio
- Posts by user
- Stats (post count, likes, etc.)

### Settings Page
- **Profile tab**: Edit name, username, email, bio, avatar
- **Password tab**: Change password
- **Account tab**: Delete account with confirmation modal

### Dashboard
- Table view of user's posts
- Edit and delete actions
- Post statistics (likes, comments)
- Quick access to create new post

### Create Post
- Rich text editor area
- Cover image upload
- Category selection
- Excerpt field
- Save draft or publish

## Tips for Using This Template

1. **Replace sample images**: Add your own images to `images/` directory
2. **Update navigation links**: Change all `href` attributes to match your structure
3. **Add backend**: This is a frontend template - connect to your backend API
4. **SEO optimization**: Add meta tags in `<head>` sections
5. **Accessibility**: The template includes ARIA labels and semantic HTML

## Static vs Dynamic

This template is **purely static** - perfect for:
- Prototyping
- Design presentations
- Learning HTML/CSS/JS
- Converting to frameworks (React, Vue, etc.)

To make it dynamic, you'll need to:
- Add a backend (Node.js, Laravel, Django, etc.)
- Set up a database
- Implement authentication
- Add API endpoints for CRUD operations

## Converting to a Framework

### React/Next.js
- Convert pages to components
- Use React Router for navigation
- Replace forms with controlled components

### Vue/Nuxt
- Create `.vue` components
- Use Vue Router
- Implement Vuex for state management

### Laravel
- Convert to Blade templates
- Use Laravel routing
- Implement Eloquent models

## License

This template is free to use for personal and commercial projects. Attribution is appreciated but not required.

## Credits

- **Design & Development**: Created from Laravel blog project
- **Typography**: Libre Baskerville (Google Fonts), Inter (Google Fonts)
- **Icons**: Heroicons (outline)

## Support

For questions or issues:
- Review the code comments in HTML/CSS files
- Check browser console for JavaScript errors
- Ensure all file paths are correct

## Changelog

### Version 1.0.0 (February 2026)
- Initial release
- All core pages and components
- Responsive design
- Sample content included

---

**Enjoy building with this template! 🚀**
