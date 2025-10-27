# TicketFlow - Vue Version

A modern, responsive ticket management application built with Vue 3 and the Composition API.

## 🚀 Features

- ✅ **Responsive Navbar** with mobile hamburger menu
- ✅ **Landing Page** with wavy hero section and decorative circles
- ✅ **Authentication** (Login/Signup) with validation
- ✅ **Dashboard** with ticket statistics
- ✅ **Full CRUD** ticket management
- ✅ **Toast Notifications** for user feedback
- ✅ **Protected Routes** with session management
- ✅ **Fully Responsive** design
- ✅ **Accessible** (ARIA labels, semantic HTML, keyboard navigation)

## 📦 Tech Stack

- **Vue 3** (Composition API)
- **Lucide Vue Next** (Icons)
- **Vite** (Build tool)
- **LocalStorage** (State persistence)
- **Scoped CSS** (Component styles)

## 🔑 Demo Credentials
```
Email: demo@test.com
Password: password123
```

## 📥 Installation

### Prerequisites
- Node.js 18+
- npm or yarn

### Setup Steps

1. **Clone the repository**
```bash
git clone TicketVue-version
cd TicketVue-version
```

2. **Install dependencies**
```bash
npm install
```

3. **Start development server**
```bash
npm run dev
```

4. **Open browser**
```
http://localhost:5174
```

## 🏗️ Project Structure
```
vue-version/
├── index.html           # HTML template (at root for Vue)
├── vite.config.js       # Vite configuration
├── package.json         # Dependencies
├── src/
│   ├── App.vue          # Main application component
│   ├── main.js          # Application entry point
│   └── components/      # Vue components
│       ├── LandingPage.vue
│       ├── LoginPage.vue
│       ├── SignupPage.vue
│       ├── Dashboard.vue
│       ├── TicketsPage.vue
│       ├── Footer.vue
│       └── WaveSVG.vue
└── README.md
```

## 📋 Available Scripts
```bash
# Development
npm run dev          # Start dev server (http://localhost:5174)

# Production
npm run build        # Build for production
npm run preview      # Preview production build

# Linting
npm run lint         # Run ESLint
```

## 🎨 Component Structure

### App.vue
- Main application component
- Handles routing and page state
- Manages authentication state
- Renders navbar and pages conditionally

### Pages
- **LandingPage.vue**: Hero section with features
- **LoginPage.vue**: User authentication form
- **SignupPage.vue**: New user registration form
- **Dashboard.vue**: Ticket statistics overview
- **TicketsPage.vue**: Full CRUD ticket management

### Components
- **Footer.vue**: Site footer
- **WaveSVG.vue**: Hero wave background

## 🔐 Authentication & Security

### Session Management
- Uses `localStorage` with key: `ticketapp_session`
- Automatically restores session on page reload
- Protected pages redirect to login if not authenticated

### Validation Rules
- **Email**: Must be valid format (name@example.com)
- **Password**: Minimum 6 characters
- **Title**: Required, minimum 3 characters
- **Status**: Must be 'open', 'in_progress', or 'closed'
- **Description**: Optional, maximum 500 characters

## 📱 Responsive Features

### Navbar
- **Desktop**: Horizontal navigation with all links visible
- **Mobile**: Hamburger menu with vertical navigation

### Layout
- **Mobile**: Single column, stacked elements
- **Tablet**: 2-column grid for stats and tickets
- **Desktop**: Multi-column layout with optimal spacing

## ♿ Accessibility

- ✅ Semantic HTML5 elements
- ✅ ARIA labels and descriptions
- ✅ Keyboard navigation support
- ✅ Focus visible states
- ✅ Error messages tied to form inputs
- ✅ Color contrast meets WCAG AA

## 🧪 State Management

### Reactive State (ref/reactive)
- Form data
- Form validation errors
- Modal visibility
- Mobile menu state

### Computed Properties
- Dashboard statistics
- Filtered ticket lists

### Persistent State (localStorage)
- Session token: `ticketapp_session`
- Tickets: `tickets` array

## 🚀 Deployment

### Build for Production
```bash
npm run build
```

### Deploy to Vercel
```bash
npm install -g vercel
vercel login
vercel --prod
```

### Deploy to Netlify

1. Build the project: `npm run build`
2. Deploy the `dist` folder
3. Add `_redirects` file: `/*    /index.html   200`

## 🐛 Troubleshooting

### Issue: Module not found
**Solution**: Run `npm install`

### Issue: Port 5174 in use
**Solution**: `npm run dev -- --port 3001`

### Issue: Tickets not persisting
**Solution**: Check localStorage in browser DevTools

### Issue: Tailwind CSS error
Solution: Delete `postcss.config.js` and remove tailwind from package.json
```bash
npm uninstall tailwindcss autoprefixer postcss
```

## 📝 LocalStorage Schema

### Session
```json
{
  "ticketapp_session": {
    "user": "demo",
    "timestamp": 1729785600000
  }
}
```

### Tickets
```json
{
  "tickets": [
    {
      "id": 1729785600000,
      "title": "Fix login bug",
      "description": "Users unable to login",
      "status": "open",
      "priority": "high",
      "createdAt": "2025-10-24T12:00:00.000Z"
    }
  ]
}
```

## 🎨 Styling Approach

- Scoped CSS in each component
- Global styles in App.vue
- Consistent design tokens (colors, spacing)
- Mobile-first responsive design

## 📞 Support

For issues or questions:
- Check this README
- Review validation rules
- Ensure demo credentials are correct
- Check browser console for errors

## 🤝 Contributing

This is a learning project for HNG Stage 2.

## 📄 License

MIT License - Educational purposes

---

Built using Vue 3 for HNG Stage 2
```

-
