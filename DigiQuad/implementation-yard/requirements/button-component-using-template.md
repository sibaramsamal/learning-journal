# ♻️ AEM Reusable Button via `data-sly-template` with Dynamic Properties

## 🧩 Objective

Build a reusable `button.html` component using `data-sly-template`, and include it in multiple AEM components (e.g., `banner.html`, `teaser.html`) with dynamic values like `color`, `title`, and `description` passed as parameters.

---

## 🗂️ File Structure

```
/apps/yourproject/components/
├── button/
│   ├── button.html        ← Reusable button template
│   └── .content.xml
├── banner/
│   ├── banner.html        ← Calls button.html with its own values
│   └── .content.xml
├── teaser/
│   ├── teaser.html        ← Also calls button.html with different values
│   └── .content.xml
```

---

## 🔁 button.html (`data-sly-template` reusable definition)

```html
<!-- /apps/yourproject/components/button/button.html -->

<template data-sly-template.button="${@ title, color, description}">
  <div class="cmp-button ${color}">
    <button class="cmp-button__btn">
      ${title}
    </button>
    <div class="cmp-button__desc">
      ${description}
    </div>
  </div>
</template>
```

> 💡 You can add `aria-*`, `id`, or other props if needed for accessibility or tracking.

---

## 📄 banner.html (uses the button template)

```html
<!-- /apps/yourproject/components/banner/banner.html -->

<div class="cmp-banner">
  <h1 class="cmp-banner__title">Banner Title</h1>
  <p class="cmp-banner__text">This is a banner content.</p>

  <!-- Include button template -->
  <div data-sly-use.buttonTemplate="yourproject.components.button.button">
    <sly data-sly-call="${buttonTemplate.button @
      title='Explore Now',
      color='primary',
      description='This button is used in Banner'}" />
  </div>
</div>
```

---

## 📄 teaser.html (uses the button template with different values)

```html
<!-- /apps/yourproject/components/teaser/teaser.html -->

<div class="cmp-teaser">
  <h2 class="cmp-teaser__headline">Teaser Headline</h2>
  <p class="cmp-teaser__summary">This is teaser summary text.</p>

  <!-- Include button template -->
  <div data-sly-use.btn="yourproject.components.button.button">
    <sly data-sly-call="${btn.button @
      title='Read More',
      color='secondary',
      description='Teaser-specific button description'}" />
  </div>
</div>
```

---

## 🎨 Example CSS (Optional)

```css
.cmp-button.primary .cmp-button__btn {
  background-color: #007bff;
  color: white;
}

.cmp-button.secondary .cmp-button__btn {
  background-color: #6c757d;
  color: white;
}
```

---

## ✅ Benefits

- 🔁 Code reuse across components
- 🎯 Dynamic parameter injection via `data-sly-template`
- 🧼 Cleaner component structure
- 🎨 Easier to style uniformly with shared CSS

---

## 🧪 Notes

- The template (`data-sly-template`) name can be anything (`button`, `renderBtn`, etc.).
- The parameters (`@ title, color, description`) are optional and can be extended.
- Use `data-sly-use` to load the template file with the correct path relative to `/apps`.

---

## 📘 References

- [HTL Template Definition & Invocation](https://www.aemtutorial.info/p/htl-sly-template-used-to-defines.html)  
- [HTL use API](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/use-api.html)

