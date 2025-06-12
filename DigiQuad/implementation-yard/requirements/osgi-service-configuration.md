# 🔧 AEM Component: DetailComponent with OSGi Config-based API Call

This component provides an input field for entering an ID and fetching JSON data from an external API, with the URL managed via OSGi configuration.

---

## 🧱 Folder Structure

```
/apps/myproject/components/detailcomponent/
├── DetailComponent.html
└── .content.xml

/core/myproject/
├── services/
│   ├── JSONAPIDetails.java
│   ├── JSONAPIDetailsImpl.java
│   └── config/
│       └── JsonApiConfig.java
└── servlets/
    └── FetchDetailsServlet.java
```

---

## 🧩 1. OSGi Configuration Interface

**`JsonApiConfig.java`**

```java
package com.myproject.core.services.config;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "JSON API Config")
public @interface JsonApiConfig {

    @AttributeDefinition(
        name = "External JSON API URL",
        description = "Base URL to fetch details (e.g., https://jsonplaceholder.typicode.com/posts/)"
    )
    String apiBaseUrl() default "https://jsonplaceholder.typicode.com/posts/";
}
```

---

## 🧠 2. Service Interface

**`JSONAPIDetails.java`**

```java
package com.myproject.core.services;

import org.json.JSONObject;

public interface JSONAPIDetails {
    JSONObject fetchjsonDetailsbyId(String id);
}
```

---

## 🧠 3. Service Implementation

**`JSONAPIDetailsImpl.java`**

```java
package com.myproject.core.services.impl;

import com.myproject.core.services.JSONAPIDetails;
import com.myproject.core.services.config.JsonApiConfig;
import org.apache.commons.io.IOUtils;
import org.json.JSONObject;
import org.osgi.service.component.annotations.*;
import org.osgi.service.metatype.annotations.Designate;

import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;

@Component(service = JSONAPIDetails.class, immediate = true)
@Designate(ocd = JsonApiConfig.class)
public class JSONAPIDetailsImpl implements JSONAPIDetails {

    private String apiBaseUrl;

    @Activate
    @Modified
    protected void activate(JsonApiConfig config) {
        this.apiBaseUrl = config.apiBaseUrl();
    }

    @Override
    public JSONObject fetchjsonDetailsbyId(String id) {
        try {
            URL url = new URL(apiBaseUrl + id);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("GET");

            InputStream is = connection.getInputStream();
            String response = IOUtils.toString(is, StandardCharsets.UTF_8);
            return new JSONObject(response);
        } catch (Exception e) {
            return new JSONObject().put("error", e.getMessage());
        }
    }
}
```

---

## 🌐 4. Sling Servlet

**`FetchDetailsServlet.java`**

```java
package com.myproject.core.servlets;

import com.myproject.core.services.JSONAPIDetails;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import java.io.IOException;

@Component(
    service = Servlet.class,
    property = {
        "sling.servlet.paths=/bin/getjsondetails",
        "sling.servlet.methods=GET"
    }
)
public class FetchDetailsServlet extends SlingAllMethodsServlet {

    @Reference
    private JSONAPIDetails jsonapiDetails;

    @Override
    protected void doGet(SlingHttpServletRequest req, SlingHttpServletResponse resp)
            throws ServletException, IOException {
        String id = req.getParameter("id");
        JSONObject data = jsonapiDetails.fetchjsonDetailsbyId(id);

        resp.setContentType("application/json");
        resp.getWriter().write(data.toString());
    }
}
```

---

## 🎨 5. HTML (HTL)

**`DetailComponent.html`**

```html
<div class="detail-component">
  <input type="text" id="input-id" placeholder="Enter ID">
  <button onclick="fetchDetails()">Submit</button>
  <pre id="result"></pre>
</div>

<script>
  function fetchDetails() {
    const id = document.getElementById("input-id").value;

    fetch("/bin/getjsondetails?id=" + id)
      .then(res => res.json())
      .then(data => {
        document.getElementById("result").textContent = JSON.stringify(data, null, 2);
      })
      .catch(error => {
        document.getElementById("result").textContent = "Error: " + error;
      });
  }
</script>
```

---

## ⚙️ 6. Configuration

Go to Web Console → `http://localhost:4502/system/console/configMgr` → search for:

> **JSON API Config**

Set the base URL to:

```
https://jsonplaceholder.typicode.com/posts/
```

---

## ✅ Output Example

User enters `1` and hits "Submit":

```json
{
  "userId": 1,
  "id": 1,
  "title": "Sample Title",
  "body": "This is a sample post body"
}
```

---

## 🔗 References

- [Writing a Sling Servlet](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/servlets/AbstractPredicateServlet.html)
- [OSGi Configuration with Metatype Annotations](https://osgi.org/specification/osgi.cmpn/7.0.0/service.component.html)
