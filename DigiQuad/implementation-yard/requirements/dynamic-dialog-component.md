# Dynamic Dialog Field System with Conditional Data Loading from JCR (AEM)

## 🧩 Objective

Implement a robust, scalable dialog system for AEM components where multiple fields are **dynamically populated from the JCR**, with **field dependencies**, **conditional logic**, and **context-awareness** across sites.

Initially only one dropdown will be there having all the categories from the given location.  
Selecting upon one category, dynamically another dropdown will display by taking that as input and fetching the respective subcategories.  
Again based on the subcategory selection, its corresponding tags should display.

If the given location is not having the desired hierarchy of nodes, it should throw a warning in the dialog itself.

After saving the details, they should persist in the JCR, and while editing, the saved data must be pre-filled. Authors should be able to edit the details as needed.

---

## 📂 JCR-Based Source Structure

Structure your dynamic data under a path like:

```
/content/mysite/data
└── categories
    ├── category-a
    │   ├── subcategories
    │   │   ├── sub-a1
    │   │   │   └── tags: [tag1, tag2]
    │   │   └── sub-a2
    └── category-b
        ├── subcategories
        │   ├── sub-b1
        │   └── sub-b2
```

Each level (category → subcategory → tags) should be a node or property structure, e.g.:

- `category-a` → `jcr:title`
- `sub-a1` → `jcr:title`, `cq:tags` (optional)

---

## 🎛️ Dialog Behavior

### 1. Category Dropdown

- **Field Type**: Granite/Coral Dropdown  
- **Source**: `/content/mysite/data/categories`  
- **Display**: `jcr:title`

### 2. Subcategory Dropdown (Dependent on Category)

- Dynamically updated via JavaScript (custom Coral event listeners or Sling Models via datasource)  
- Hidden until a category is selected

### 3. Tags/Metrics Display (Dependent on Subcategory)

- Multi-select or display-only field  
- Fetch and show tags or metadata from selected subcategory node

---

## 👁️ Conditional Field Visibility

- Use `granite:renderCondition`, `cq:showOn`, or custom JS logic  
- Examples:  
  - Show "Tags" field **only** if a specific subcategory is selected  
  - Show "Advanced Options" panel if `category == "Advanced"`

---

## 🔐 Permission-Sensitive Data

Use a **custom Sling Servlet** or **Sling Model** that:

- Reads data from JCR  
- Filters nodes based on `ResourceResolver.hasPermission()`  
- Ensures authors only see what they’re permitted to

---

## 🌍 Multi-Site Aware Support

- Automatically detect site context (e.g., `/content/us`, `/content/de`)  
- Load data from the corresponding site's `/data` node  
- Use `SlingBindings` or `PageManager` to determine the root path dynamically

---

## ⚡ Caching Optimization

- Cache data at request or session level during authoring  
- Use `@RequestScoped` Sling Models or lightweight service-layer caching (e.g., Guava Cache)  
- Minimize JCR reads on dialog render

---

## 🛑 Fallback Behavior

If data is misconfigured or missing:

- Show fallback static options (e.g., `Default Category`)  
- Display inline warning using Granite UI

Example:

```xml
<granite:alert
  jcr:primaryType="nt:unstructured"
  variant="error"
  text="Category data is missing or not configured properly." />
```

---

## 🧾 Deliverables

- ✅ A fully functional AEM component dialog with:
  - Dynamic dropdowns (categories → subcategories → tags)
  - Context-aware and permission-sensitive behavior
  - Multi-site adaptability
  - Graceful fallback/error messaging

- ✅ Configuration documentation:
  - JCR path expectations
  - Required node types/properties
  - Sample JCR structure

- ✅ Admin-friendly UX and inline warnings

- 🔁 **Optional**: Analytics/logging of author selections (for auditing or insight)

---

## 🛠️ Technology Stack

- AEM 6.5+ or AEM as a Cloud Service  
- Granite/Coral UI (Dialog)  
- Sling Models (backend population)  
- JavaScript (event listeners for dynamic updates)  
- OSGi caching services (if needed)  
- Custom Sling Servlet (optional permissions/data filter)

---

## 📘 References

- [Granite UI Docs](https://developer.adobe.com/experience-manager/reference-materials/6-5/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/index.html)
- [Sling Models Guide](https://sling.apache.org/documentation/bundles/models.html)
- [Dynamic Dropdown in AEM Touch UI Dialog (Medium)](https://medium.com/@arunpatidar26/dynamic-dropdown-in-aem-touch-ui-cc502022da24)