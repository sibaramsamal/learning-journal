# ✅ AEM Component Creation with Client Library (Best Practices)

## 🎯 Objective

Establish a standardized and efficient approach to creating AEM components using Client Libraries for styling, scripting, and modular reuse. This guide outlines the setup of `clientlibs`, their properties, and their role in dialog functionality, styling, and component interactivity.

## 🧱 Component Folder Structure

```
/apps/yourproject/components/your-component/
├── clientlibs
│   ├── css.txt
│   ├── js.txt
│   ├── script.js
│   ├── style.css
│   └── _cq_clientlibs
│       └── .content.xml
├── dialog
│   └── dialog.xml
├── your-component.html
└── .content.xml
```

## 📦 Client Library Configuration

The `clientlibs` folder encapsulates JavaScript and CSS specific to your component.

### _cq_clientlibs/.content.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root
    xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="cq:ClientLibraryFolder"
    categories="[yourproject.components.your-component]"
    embed="[yourproject.base]"
    dependencies="[granite.jquery, cq.authoring.dialog]"
    allowProxy="{Boolean}true" />
```

## 📜 css.txt and js.txt

### css.txt

```
style.css
```

### js.txt

```
script.js
```

## 🎨 style.css

```css
.cmp-your-component {
  padding: 16px;
  border: 1px solid #ccc;
}
```

## ⚙️ script.js

```js
(function(document) {
  document.addEventListener("DOMContentLoaded", function() {
    const el = document.querySelector(".cmp-your-component");
    if (el) {
      console.log("Component loaded:", el);
    }
  });
})(document);
```

## 🎛️ Dialog Enhancements (Optional)

```js
$(document).on("dialog-ready", function() {
  const categoryField = $("coral-select[name='./category']");
  categoryField.on("change", function() {
    console.log("Category changed:", $(this).val());
  });
});
```

## 📥 Including the Clientlib

### Option 1: Via HTL (component level)

```html
<head data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
      data-sly-call="${clientLib.all @ categories='yourproject.components.your-component'}">
</head>
```

### Option 2: Via Page Template

```html
<meta data-sly-use.clientlib="/libs/granite/sightly/templates/clientlib.html"
      data-sly-call="${clientlib.all @ categories='yourproject.components.your-component'}"/>
```

## 📘 Best Practices

- ✅ Use namespaced class names (`cmp-your-component`) to avoid style conflicts.
- ✅ Always set `allowProxy=true` in `.content.xml`.
- ✅ Use `embed` for shared/global clientlibs like `yourproject.base`.
- ✅ Keep component clientlibs small and modular.
- ✅ Use `dependencies` for required libs (e.g., jQuery, dialog).

## ✅ Deliverables Checklist

- [x] Component-specific client library
- [x] categories, dependencies, embed configured
- [x] `.txt` files referencing styles/scripts
- [x] Scoped CSS classes (`cmp-*`)
- [x] Optional: JS logic for dialog enhancement
- [x] Clientlib loaded via HTL/template

## 📚 References

- [AEM Core Components - ClientLibs](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/introduction/clientlibs)
- [Granite UI](https://developer.adobe.com/experience-manager/reference-materials/6-5/granite-ui/api/)
- [Coral UI Events](https://opensource.adobe.com/coral-spectrum/documentation/)
