---
created: 2024-07-07T12:55:53 (UTC -03:00)
tags: [http,javascript,development,software,coding,development,engineering,inclusive,community]
source: https://dev.to/wafa_bergaoui/axios-vs-fetch-543c?ref=dailydev
author: 
---

# Axios vs Fetch - DEV Community

> ## Excerpt
> Introduction   In modern web development, making HTTP requests is a fundamental task....

---
## [](https://dev.to/wafa_bergaoui/axios-vs-fetch-543c?ref=dailydev#introduction)**Introduction**

In modern web development, making HTTP requests is a fundamental task. Whether you're fetching data from a server, submitting forms, or interacting with APIs, you need reliable tools to handle these operations. While JavaScript provides a built-in fetch API for making HTTP requests, many developers opt for third-party libraries like Axios for added functionality and convenience.

## [](https://dev.to/wafa_bergaoui/axios-vs-fetch-543c?ref=dailydev#why-do-we-need-http-request-tools)**Why Do We Need HTTP Request Tools?**

Handling HTTP requests and responses can be complex, especially when considering error handling, response parsing, and request configuration. Tools like Axios and Fetch simplify these tasks by providing abstractions and utilities that streamline the process. They help address common problems such as:

**Boilerplate Code:** Simplifying repetitive tasks like setting headers and handling JSON responses.  
**Error Handling:** Providing more consistent and manageable error handling mechanisms.  
**Interceptors:** Allowing pre-processing of requests or responses, such as adding authentication tokens.

## [](https://dev.to/wafa_bergaoui/axios-vs-fetch-543c?ref=dailydev#fetch-api)**Fetch API**

The Fetch API is a modern, built-in JavaScript method for making HTTP requests. It is promise-based, providing a more straightforward way to work with asynchronous operations compared to older methods like XMLHttpRequest.

**Example**  

```
// Making a GET request using Fetch
fetch('https://api.example.com/data')
  .then(response =&gt; {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
  .then(data =&gt; console.log(data))
  .catch(error =&gt; console.error('There was a problem with the fetch operation:', error));

```

## [](https://dev.to/wafa_bergaoui/axios-vs-fetch-543c?ref=dailydev#axios)**Axios**

Axios is a popular third-party library for making HTTP requests. It is promise-based like Fetch but includes many additional features that make it more convenient and powerful.

**Example**  

```
// Making a GET request using Axios
axios.get('https://api.example.com/data')
  .then(response =&gt; console.log(response.data))
  .catch(error =&gt; console.error('There was a problem with the axios request:', error));

```

## [](https://dev.to/wafa_bergaoui/axios-vs-fetch-543c?ref=dailydev#key-differences)**Key Differences**

**1\. Default Handling of JSON**

**\- Fetch:**  
Requires manual conversion of response data to JSON.  

```
fetch('https://api.example.com/data')
  .then(response =&gt; response.json()) // Manual conversion
  .then(data =&gt; console.log(data));

```

**\- Axios:**  
Automatically parses JSON responses.  

```
axios.get('https://api.example.com/data')
  .then(response =&gt; console.log(response.data)); // Automatic conversion

```

**2\. Error Handling**

**\- Fetch:**  
Only rejects a promise for network errors, not for HTTP errors (e.g., 404 or 500 status codes).  

```
fetch('https://api.example.com/data')
  .then(response =&gt; {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
  .catch(error =&gt; console.error('Fetch error:', error));

```

**\- Axios:**  
Rejects a promise for both network errors and HTTP errors.  

```
axios.get('https://api.example.com/data')
  .catch(error =&gt; console.error('Axios error:', error));

```

**3\. Request Configuration**

**\- Fetch:**  
Requires manual configuration of options like headers and method  

```
fetch('https://api.example.com/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ key: 'value' })
});

```

**\- Axios:**  
Provides a more concise and readable syntax for configuration.  

```
axios.post('https://api.example.com/data', { key: 'value' }, {
  headers: {
    'Content-Type': 'application/json'
  }
});

```

## [](https://dev.to/wafa_bergaoui/axios-vs-fetch-543c?ref=dailydev#interceptors)**Interceptors**

Interceptors are a powerful feature in Axios that allow you to intercept requests or responses before they are handled by then or catch. They are useful for tasks like adding authentication tokens to every request or handling errors globally.

**Example of Axios Interceptors**  

```
// Adding a request interceptor
axios.interceptors.request.use(config =&gt; {
  // Add an authorization header to every request
  const token = 'your_token';
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
}, error =&gt; {
  // Handle request error
  return Promise.reject(error);
});

// Adding a response interceptor
axios.interceptors.response.use(response =&gt; {
  // Handle successful response
  return response;
}, error =&gt; {
  // Handle response error
  if (error.response.status === 401) {
    // Handle unauthorized error
    console.error('Unauthorized');
  }
  return Promise.reject(error);
});

```

## [](https://dev.to/wafa_bergaoui/axios-vs-fetch-543c?ref=dailydev#conclusion)**Conclusion**

Both Fetch and Axios are capable tools for making HTTP requests, but they offer different levels of convenience and functionality. Fetch is a native, modern option that works well for simple use cases, while Axios provides a richer feature set, including automatic JSON handling, better error management, and interceptors for request and response manipulation. Choosing between them depends on the specific needs of your project and your preference for simplicity versus functionality.

By understanding the strengths and differences of each tool, you can make more informed decisions and write more efficient and maintainable code.
