# 🔍 AEM DetailComponent - Fetch JSON by ID from External API

## 🧩 Objective

Create an AEM component `DetailComponent` with an input field. When a user submits an ID, the component should call an external API (`https://jsonplaceholder.typicode.com/`) using Java, fetch JSON data, and display it in the component output.

---

## 🗂️ Structure

```
/apps/<project>/components/detailcomponent/
├── detailcomponent.html
├── clientlibs/
│   ├── js.txt
│   └── script.js
├── dialog/
│   └── dialog.xml
└── .content.xml

/core/<project>/services/
├── JsonAPIDetails.java
├── impl/
│   └── JsonAPIDetailsImpl.java

/core/<project>/servlets/
└── FetchJSONServlet.java
```

---

## 📄 detailcomponent.html

```html
<div class="detail-component">
  <input type="text" id="input-id" placeholder="Enter ID">
  <button onclick="fetchDetails()">Submit</button>
  <pre id="result"></pre>
</div>
```

---

## 📜 script.js

```javascript
function fetchDetails() {
  const id = document.getElementById("input-id").value;

  fetch("/bin/getjsondetails?id=" + id)
    .then(res => res.json())
    .then(data => {
      document.getElementById("result").innerText = JSON.stringify(data, null, 2);
    })
    .catch(err => {
      document.getElementById("result").innerText = "Error fetching data: " + err;
    });
}
```

---

## 📄 js.txt

```
script.js
```

---

## 💬 dialog.xml

```xml
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
          xmlns:cq="http://www.day.com/jcr/cq/1.0"
          xmlns:jcr="http://www.jcp.org/jcr/1.0"
          xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
          jcr:primaryType="nt:unstructured"
          jcr:title="Detail Component"
          sling:resourceType="cq/gui/components/authoring/dialog">
  <content
      jcr:primaryType="nt:unstructured"
      sling:resourceType="granite/ui/components/coral/foundation/container">
    <items jcr:primaryType="nt:unstructured">
      <field
          jcr:primaryType="nt:unstructured"
          sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
          name="./id"
          fieldLabel="ID"
          required="true"/>
    </items>
  </content>
</jcr:root>
```

---

## 📄 JsonAPIDetails.java

```java
package com.myproject.core.services;

import org.json.JSONObject;

public interface JsonAPIDetails {
    JSONObject fetchjsonDetailsbyId(String id);
}
```

---

## 📄 JsonAPIDetailsImpl.java

```java
package com.myproject.core.services.impl;

import com.myproject.core.services.JsonAPIDetails;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

@Component(service = JsonAPIDetails.class)
public class JsonAPIDetailsImpl implements JsonAPIDetails {

    @Override
    public JSONObject fetchjsonDetailsbyId(String id) {
        JSONObject responseJSON = new JSONObject();
        try {
            URL url = new URL("https://jsonplaceholder.typicode.com/posts/" + id);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");

            BufferedReader in = new BufferedReader(
                new InputStreamReader(conn.getInputStream())
            );
            StringBuilder response = new StringBuilder();
            String inputLine;
            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            in.close();

            responseJSON = new JSONObject(response.toString());
        } catch (Exception e) {
            responseJSON.put("error", e.getMessage());
        }

        return responseJSON;
    }
}
```

---

## 📄 FetchJSONServlet.java

```java
package com.myproject.core.servlets;

import com.myproject.core.services.JsonAPIDetails;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import javax.servlet.Servlet;
import java.io.IOException;

@Component(
    service = Servlet.class,
    property = {
        "sling.servlet.paths=/bin/getjsondetails",
        "sling.servlet.methods=GET"
    }
)
public class FetchJSONServlet extends SlingAllMethodsServlet {

    @Reference
    private JsonAPIDetails jsonAPIDetails;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        String id = request.getParameter("id");
        JSONObject result = jsonAPIDetails.fetchjsonDetailsbyId(id);

        response.setContentType("application/json");
        response.getWriter().write(result.toString());
    }
}
```

---

## ✅ Output

- Authors can enter an ID in the field and submit.
- The JSON response for the respective ID is fetched from `https://jsonplaceholder.typicode.com/posts/{id}`.
- The result is displayed below the input box.

---

## 📘 References

- [JSONPlaceholder API](https://jsonplaceholder.typicode.com/)
- [Writing a Sling Servlet in Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-sling.html)
- [HTL Use-API Guide](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/use-api.html)

