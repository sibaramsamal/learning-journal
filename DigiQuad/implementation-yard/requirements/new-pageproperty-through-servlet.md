# 🛠️ AEM Resource Type-based Servlet to Add and Display a JCR Property

This component demonstrates a **resource-type servlet** that adds a property (e.g., `additionalProperty`) to an existing AEM page and displays it via the HTL component.

---

## 📂 Folder Structure

```
/apps/myproject/components/detailcomponent/
├── DetailComponent.html
└── .content.xml

/core/myproject/
└── servlets/
    └── AddPropertyServlet.java
```

---

## ⚙️ 1. Servlet (Resource Type-based)

**`AddPropertyServlet.java`**

```java
package com.myproject.core.servlets;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ModifiableValueMap;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;

import javax.jcr.Session;
import javax.servlet.Servlet;
import javax.servlet.ServletException;
import java.io.IOException;

@Component(
    service = Servlet.class,
    property = {
        "sling.servlet.resourceTypes=myproject/components/detailcomponent",
        "sling.servlet.methods=POST",
        "sling.servlet.extensions=json"
    }
)
public class AddPropertyServlet extends SlingAllMethodsServlet {

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        Resource resource = request.getResource();
        ModifiableValueMap map = resource.adaptTo(ModifiableValueMap.class);

        if (map != null) {
            map.put("additionalProperty", "Added by Servlet");
            resource.getResourceResolver().commit();

            response.setContentType("application/json");
            response.getWriter().write("{\"message\": \"Property added successfully.\"}");
        } else {
            response.setStatus(SlingHttpServletResponse.SC_INTERNAL_SERVER_ERROR);
            response.getWriter().write("{\"error\": \"Unable to adapt resource.\"}");
        }
    }
}
```

---

## 🎨 2. Component HTL

**`DetailComponent.html`**

```html
<div class="detail-component">
  <button onclick="addProperty()">Add Property</button>
  <p><strong>Additional Property:</strong> ${properties.additionalProperty}</p>
</div>

<script>
  function addProperty() {
    fetch(window.location.pathname + ".json", {
      method: "POST"
    })
    .then(res => res.json())
    .then(data => {
      alert(data.message || data.error);
      location.reload(); // Reload to reflect added property
    });
  }
</script>
```

---

## 📝 3. Component Definition

**`.content.xml` (for component registration)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root
    xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="cq:Component"
    jcr:title="Detail Component"
    componentGroup="MyProject"
    sling:resourceSuperType="core/wcm/components/page/v2/page"
    sling:resourceType="myproject/components/detailcomponent"/>
```

---

## 🔄 4. Usage Instructions

1. Drag and drop `DetailComponent` onto any page.
2. Click the **“Add Property”** button.
3. Servlet writes `additionalProperty = "Added by Servlet"` to the page node.
4. Page reloads and shows the property value.

---

## ✅ Expected Output

```
Additional Property: Added by Servlet
```

---

## 📌 Notes

- Servlet is bound to the component via `sling.servlet.resourceTypes`.
- It only triggers for POST requests (e.g., form submission, button click).
- Uses `ModifiableValueMap` to write property and `ResourceResolver.commit()` to save.
- Reloading the page shows the new value from JCR via `properties.additionalProperty`.

---

## 🔗 Reference

- [Sling Resource Type Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#servlet-registration)
