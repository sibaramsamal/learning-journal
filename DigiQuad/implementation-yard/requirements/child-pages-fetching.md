# 📘 Fetching Child Page Properties from a Dialog Path in AEM

This guide explains how to read a **path input from a dialog**, convert it to a valid JCR resource/page, and extract properties of its **child pages**, using **two approaches**:

---

## 🎯 Objective

- Read a path entered via component dialog (e.g., `/content/mysite/home`)
- Convert it into a valid JCR resource
- Traverse its **immediate child pages**
- Fetch properties like `jcr:title`, `cq:tags`, etc.
- Support both `ResourceResolver` and `PageManager` based techniques

---

## 📦 Folder Structure (Simplified)

```
/apps/myproject/components/pagepropertiesfetcher/
├── pageproperties.html
└── .content.xml

/core/myproject/servlets/
└── PagePropertyServlet.java
```

---

## 📑 Dialog Configuration Example

```xml
<contentPath
    jcr:primaryType="nt:unstructured"
    sling:resourceType="granite/ui/components/coral/foundation/form/pathfield"
    name="./targetPath"
    fieldLabel="Parent Page Path"
    rootPath="/content"
    required="true" />
```

---

## 🧠 Sling Model or Servlet

### ✅ `PagePropertyServlet.java`

```java
package com.myproject.core.servlets;

import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.PageManager;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONArray;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import java.io.IOException;
import java.util.Iterator;

@Component(
    service = Servlet.class,
    property = {
        "sling.servlet.paths=/bin/fetchchildproperties",
        "sling.servlet.methods=GET"
    }
)
public class PagePropertyServlet extends SlingAllMethodsServlet {

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {

        String path = request.getParameter("path"); // e.g., from dialog
        JSONObject result = new JSONObject();
        JSONArray childrenArray = new JSONArray();

        ResourceResolver resolver = request.getResourceResolver();

        try {
            // ─── Approach 1: Using ResourceResolver ───
            Resource parentResource = resolver.getResource(path);

            if (parentResource != null) {
                Iterator<Resource> children = parentResource.listChildren();
                while (children.hasNext()) {
                    Resource child = children.next();
                    String title = child.getValueMap().get("jcr:title", child.getName());

                    JSONObject obj = new JSONObject();
                    obj.put("name", child.getName());
                    obj.put("title", title);
                    childrenArray.put(obj);
                }
            }

            // ─── Approach 2: Using PageManager ───
            PageManager pageManager = resolver.adaptTo(PageManager.class);
            Page parentPage = pageManager.getPage(path);

            if (parentPage != null) {
                Iterator<Page> childPages = parentPage.listChildren();
                while (childPages.hasNext()) {
                    Page child = childPages.next();
                    JSONObject obj = new JSONObject();
                    obj.put("pageName", child.getName());
                    obj.put("pageTitle", child.getTitle());
                    childrenArray.put(obj);
                }
            }

            result.put("children", childrenArray);

        } catch (Exception e) {
            result.put("error", e.getMessage());
        }

        response.setContentType("application/json");
        response.getWriter().write(result.toString());
    }
}
```

---

## 💻 HTL Example (pageproperties.html)

```html
<div>
  <input type="text" id="pathInput" placeholder="Enter page path" />
  <button onclick="fetchProps()">Fetch Properties</button>

  <pre id="output"></pre>
</div>

<script>
  function fetchProps() {
    const path = document.getElementById("pathInput").value;

    fetch(`/bin/fetchchildproperties?path=${encodeURIComponent(path)}`)
      .then(res => res.json())
      .then(data => {
        document.getElementById("output").innerText = JSON.stringify(data, null, 2);
      });
  }
</script>
```

---

## 📝 Summary of Approaches

| Approach | Method | Usage |
|---------|--------|-------|
| 1 | `ResourceResolver.getResource()` → `listChildren()` | Useful when working with raw JCR nodes |
| 2 | `PageManager.getPage()` → `listChildren()` | Ideal when you want to leverage AEM’s page APIs (title, navigation, etc.) |

---

## ✅ Expected Output (Example)

```json
{
  "children": [
    {
      "name": "about",
      "title": "About Us"
    },
    {
      "pageName": "products",
      "pageTitle": "Our Products"
    }
  ]
}
```

---

## 🔗 References

- [Page API – Adobe](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/Page.html)
- [ResourceResolver Docs](https://sling.apache.org/apidocs/sling9/org/apache/sling/api/resource/ResourceResolver.html)

