## **Mobile-First CSS**

In 2026, designing CSS to be
mobile-first is the industry standard. It is a strategic approach where you style for the smallest screens first and then "layer on" complexity for larger screens using media queries. 
Why You Should Design Mobile-First

- **Performance**: Mobile-first sites are typically 30–50% faster. Devices don't have to "undo" heavy desktop styles or download unnecessary high-resolution assets before rendering the page.
- **SEO Advantage**: Google uses mobile-first indexing, meaning it primarily evaluates your mobile version to determine your ranking across all devices.
- **Content Focus**: Constraints force you to prioritize essential content and clear navigation, resulting in a cleaner user experience (UX) for everyone.
- **Easier Maintenance**: It is technically easier to "grow" a design from a simple stack into a complex grid than to "shrink" a complex desktop layout into a tiny screen. 

### **How to Implement It (CSS Strategy)**
The core of mobile-first CSS is using min-width media queries. This ensures that your base styles apply to everything, and specialized styles only "kick in" when the screen is wide enough. 

- **Write Base Styles**: These are your "default" styles for the smallest screens (0px and up). No media query is needed here.
- **Add Breakpoints**: 
    - Use min-width to add layouts for larger devices.
    - Tablet: @media (min-width: 768px) { ... }
    - Desktop: @media (min-width: 1024px) { ... } 

#### **Example Comparison**
Approach 
**Logic             Media Query Used**
Mobile-First	    Default is mobile; add for 
                    large screen (min-width)
Desktop-First	    Default is desktop; remove for 
                    small screens (max-width)

#### **Common Principles for 2026**

- **Touch-Friendly Targets**: Buttons and interactive elements should be at least 44x44 or 48x48 pixels to accommodate human fingers.
- **Avoid Hovers**: Do not rely on hover effects for essential navigation, as they do not work on touchscreens.
- **Fluid Layouts**: Use relative units like %, rem, vw, and vh instead of fixed pixel widths to ensure your design adapts to the massive variety of modern devices. 

### **making animated hamburger nav menu**
#### **"Checkbox Hack"**
To create a CSS-only show/hide menu, you must use the "Checkbox Hack". This technique involves adding a hidden checkbox to the HTML to store the "open/closed" state, then using CSS to toggle the menu's visibility when the box is checked.

1. Updated HTML
Add an `<input type="checkbox">` and wrap your burger icon in a `<label>`. The id on the input must match the for on the label so clicking the icon toggles the checkbox.

```html
<nav>
    <a href="index.html">
        <img src="img/tahoe-logo.svg" alt="Tahoe Adventure Club">
    </a>

    <!-- 1. Hidden checkbox to hold the state -->
    <input type="checkbox" id="nav-toggle" class="nav-toggle">

    <!-- 2. Label acts as the clickable trigger for the checkbox -->
    <label for="nav-toggle" class="burger-label">
        <img src="img/burger-menu.svg" alt="hamburger menu nav icon" class="burger">
    </label>

    <ul>
        <li><a href="things-to-do.html">Things To Do</a></li>
        <li><a href="places-to-stay.html">Stay</a></li>
        <li><a href="food-drink.html">Food & Drink</a></li>
    </ul>
</nav>
```

2. Mobile-First CSS
This CSS hides the menu by default and shows it only when the checkbox is "checked". In 2026, using flex or grid for layout is the standard for responsive design.

```css
/* --- MOBILE BASE STYLES --- */

/* Hide the actual checkbox from view */
.nav-toggle {
    display: none;
}

/* Base style for the list (hidden) */
ul {
    display: none;
    flex-direction: column; /* Vertical stack for mobile */
    width: 100%;
}

.burger {
    width: 44px; /* Touch-friendly size */
    cursor: pointer;
}

/* THE MAGIC: When checkbox is checked, show the adjacent ul */
.nav-toggle:checked ~ ul {
    display: flex;
}

/* --- DESKTOP (1024px+) --- */
@media (min-width: 1024px) {
    /* Hide the burger trigger */
    .burger-label {
        display: none;
    }

    /* Force show the menu as a horizontal row */
    ul {
        display: flex;
        flex-direction: row;
        width: auto;
    }
}
```

Why this works:

- **The Tilde (~) Selector**: The ~ symbol is the "subsequent-sibling combinator." It tells CSS: "Look for a <ul> that comes after a checked `.nav-toggle` and change its display".
- **Zero JavaScript**: Because the checkbox natively maintains an "on/off" state, you don't need a single line of script to make it work.
- **Mobile-First logic**: The <ul> starts as `display: none`. You only write the extra code to show it and layout the desktop version inside the @media query.

### **interpolate-size: allow-keywords:**
Without this line, the menu would simply "snap" open instantly. This property tells the browser to calculate the math between 0 and auto.

- **overflow: hidden***: This is essential on the <ul>. It ensures that the text inside doesn't "peek out" while the height is still animating from 0.
- **flex-wrap**: wrap: By setting the <nav> to wrap, the <ul> (which is width: 100%) will naturally drop to its own line below the logo and burger icon when it expands.
- **No JavaScript**: This entire interaction is handled by the browser's rendering engine, which is more performant and accessible than script-based height calculations. 