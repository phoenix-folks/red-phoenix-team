# Red Phoenix Team - Landing Page

A premium one-page landing site for Red Phoenix Team, a software development agency.

## Features

- **Dark Tech Forge Design** - Sophisticated dark theme with vibrant phoenix orange accents
- **Animated Grid Background** - Subtle pulsing grid animation that creates an ambient atmosphere
- **Responsive Design** - Mobile-first approach with seamless experience across all devices
- **Interactive Contact Form** - Integrated with getform.io for reliable message delivery
- **Toast Notifications** - Elegant feedback system for form submissions
- **Smooth Animations** - Hover effects, transitions, and scroll behavior
- **Accessibility** - Proper semantic HTML, heading hierarchy, and keyboard navigation

## Tech Stack

- HTML5
- Tailwind CSS (latest CDN)
- Vanilla JavaScript
- Google Fonts (JetBrains Mono + IBM Plex Sans)

## Deployment to GitHub Pages

1. Go to your repository on GitHub
2. Click on **Settings** tab
3. Scroll down to **Pages** section in the left sidebar
4. Under **Source**, select **main** branch
5. Keep the folder as **/ (root)**
6. Click **Save**
7. Wait a few minutes for deployment
8. Your site will be live at: `https://yourusername.github.io/red-phoenix-team/`

## Customization

### Update Team Members

Edit the team member sections in `index.html` (search for "Team Member 1", etc.):

```html
<h3>Your Name</h3>
<p>Your Position</p>
<a href="https://linkedin.com/in/your-profile">LinkedIn</a>
```

### Update Contact Email

Replace `hello@red-phoenix.team` in the footer section with your actual email.

### Update Social Links

Replace the LinkedIn company URL in the footer:

```html
<a href="https://linkedin.com/company/your-company">
```

### Modify Colors

Update CSS variables in the `<style>` section:

```css
:root {
    --bg-primary: #0a0a0f;
    --bg-secondary: #12121a;
    --accent-orange: #ff4d00;
    --accent-red: #ff1744;
}
```

## Form Integration

The contact form is integrated with getform.io. To use your own endpoint:

1. Sign up at [getform.io](https://getform.io)
2. Create a new form
3. Copy your form endpoint
4. Replace the endpoint in the fetch request:

```javascript
const response = await fetch('YOUR_GETFORM_ENDPOINT', {
    method: 'POST',
    body: formData
});
```

## License

All rights reserved - Red Phoenix Team 2025

## Credits

- Design & Development: Red Phoenix Team
- Icons: SVG custom graphics
- Fonts: Google Fonts (JetBrains Mono, IBM Plex Sans)
- Avatars: UI Avatars API