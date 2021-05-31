# 🌐 REST API Design

## Contents

- [Naming your Endpoints](#naming-your-endpoints)
  - [Define operations with HTTP Methods](#define-operations-with-http-methods)
- [Return Status Code](#return-status-code)
- [Parameters](#parameters)
- [Versioning](#versioning)
- [HATEOAS](#hateoas)
- [Error Payload](#error-payload)
- [Security](#security)
  - [Keep it Simple](#keep-it-simple)
  - [Always Use HTTPS](#always-use-https)
  - [Use Password Hash](#use-password-hash)
  - [Never expose information on URLs](#never-expose-information-on-urls)
- [References](#references)

## Naming your Endpoints

Focus on resources, business entities that you will expose. Use always nouns in plural for that, avoid using actions or verbs. Always prefer to use `kebab-case` (lower case with dashes) in endpoint naming as it's a widely adopted practice since [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) defines URIs as case-sensitive except for the scheme and host components.

E.g.:

- `/orders` ✔️
- `/managed-devices` ✔️
- `/ManagedDevices` ❌ (no pascal/camel case)
- `/create-order` ❌ (no verbs)

### Define operations with HTTP Methods

The HTTP protocol defines a number of methods that assign semantic meaning to a request. The common HTTP methods used by most RESTful web APIs are:

- `GET`- retrieves a representation of the resource at the specified URI. The body of the response message contains the details of the requested resource.
- `POST`- creates a new resource at the specified URI. The body of the request message provides the details of the new resource. Note that POST can also be used to trigger operations that don't actually create resources.
- `PUT`- either creates or replaces the resource at the specified URI. The body of the request message specifies the resource to be created or updated.
- `DELETE` - removes the resource at the specified URI.

## Return Status Code

To eliminate confusion for API users when an error occurs, we should handle errors gracefully and return HTTP response codes that indicate what kind of error occurred. This gives maintainers of the API enough information to understand the problem that’s occurred. We don’t want errors to bring down our system, so we can leave them unhandled, which means that the API consumer has to handle them.

Common error HTTP status codes include:

- `400 Bad Request`- This means that client-side input fails validation.
- `401 Unauthorized`- This means the user isn’t not authorized to access a resource. It usually returns when the user isn’t authenticated.
- `403 Forbidden`- This means the user is authenticated, but it’s not allowed to access a resource.
- `404 Not Found`- This indicates that a resource is not found.
- `500 Internal server error`- This is a generic server error. It probably shouldn’t be thrown explicitly.
- `502 Bad Gateway`- This indicates an invalid response from an upstream server.
- `503 Service Unavailable`- This indicates that something unexpected happened on server side (It can be anything like server overload, some parts of the system failed, etc.).

We should be throwing errors that correspond to the problem that our app has encountered. For example, if we want to reject the data from the request payload, then we should return a 400 response.

Click [here](https://httpstatuses.com/) to see more Http Status.

## Parameters

When sorting, searching, filtering or pagining, use querystring parameters. All of these actions are simply the query on one dataset. There will be no new set of APIs to handle these actions. We need to append the query params with the GET method API.
Let’s understand with few examples how to implement these actions.

- **Sorting** In case, the client wants to get the sorted list of companies, the GET /companies endpoint should accept multiple sort params in the query.
  - `GET /companies?sort=rank_asc` would sort the companies by its rank in ascending order.
- **Filtering** For filtering the dataset, we can pass various options through query params.
  - `GET /companies?category=banking&location=india` would filter the companies list data with the company category of Banking and where the location is India.
- **Searching** When searching the company name in companies list the API endpoint
  - `GET /companies?search=Digital%20Mckinsey`
- **Pagination** When the dataset is too large, we divide the data set into smaller chunks, which helps in improving the performance and is easier to handle the response.
  - `GET /companies?page=23` means get the list of companies on 23rd page.

## Versioning

It is highly unlikely that a web API will remain static. We should have different versions of API if we’re making any changes to them that may break clients. The versioning can be done according to semantic version (for example, 2.0.6 to indicate major version 2 and the sixth patch) like most apps do nowadays.

This way, we can gradually phase out old endpoints instead of forcing everyone to move to the new API at the same time. The v1 endpoint can stay active for people who don’t want to change, while the v2, with its shiny new features, can serve those who are ready to upgrade. This is especially important if our API is public. We should version them so that we won’t break third party apps that use our APIs.

Versioning is usually done with `/v1/`, `/v2/`, etc. added at the start of the API path.

So `/orders` become `/v1/orders` when you add versions to it.

## HATEOAS

**Hypermedia as the Engine of Application State** is a constraint of the REST application architecture that keeps the RESTful style architecture unique from most other network application architectures. The term “_**hypermedia**_” refers to any content that contains links to other forms of media such as images, movies, and text.

REST architectural style lets you use hypermedia links in the response contents so that the client can dynamically navigate to the appropriate resource by traversing the hypermedia links. Above is conceptually the same as a web user browsing through web pages by clicking the relevant hyperlinks to achieve a final goal.

For example, to handle the relationship between an order and a customer, the representation of an order could include links that identify the available operations for the customer of the order. Here is a possible representation:

```json
{
  "orderID":3,
  "productID":2,
  "quantity":4,
  "orderValue":16.60,
  "links":[
    {
      "rel":"customer",
      "href":"https://adventure-works.com/customers/3",
      "action":"GET",
      "types":["text/xml","application/json"]
    },
    {
      "rel":"customer",
      "href":"https://adventure-works.com/customers/3",
      "action":"PUT",
      "types":["application/x-www-form-urlencoded"]
    },
    {
      "rel":"customer",
      "href":"https://adventure-works.com/customers/3",
      "action":"DELETE",
      "types":[]
    },
    {
      "rel":"self",
      "href":"https://adventure-works.com/orders/3",
      "action":"GET",
      "types":["text/xml","application/json"]
    },
    {
      "rel":"self",
      "href":"https://adventure-works.com/orders/3",
      "action":"PUT",
      "types":["application/x-www-form-urlencoded"]
    },
    {
      "rel":"self",
      "href":"https://adventure-works.com/orders/3",
      "action":"DELETE",
      "types":[]
    }]
}
```

## Error Payload

Wrap your responses in an Error Payload structure, sometimes it is easy to return a problem, for instance, your system only accepts one entry per e-mail and someone is trying to add the same e-mail again, you can return 400 Bad Request you're fine but what if there is an error in a business logic that you can not related to a HTTP Status? So, if you have an error payload that would deal with that, just return 200 to indicate that the request is fine but have a field indicating success and error messages when it fails:

```json
{
    "success" : "false",
    "error":{
        "code":"123",
        "message":"Something went wrong"
    }
}

```

And when it succeed it would be like this:

```json
{
    "success" : "true",
    "data": {
        ... your response
    }
}
```

## Security

### Keep it Simple

Secure an API/System – just how secure it needs to be. Every time you make the solution more complex “unnecessarily,” you are also likely to leave a hole.

### Always Use HTTPS

By always using SSL, the authentication credentials can be simplified to a randomly generated access token that is delivered in the username field of HTTP Basic Auth. It’s relatively simple to use, and you get a lot of security features for free.

If you use HTTP 2, to improve performance – you can even send multiple requests over a single connection, that way you avoid the complete TCP and SSL handshake overhead on later requests.

### Use Password Hash

Passwords must always be hashed to protect the system (or minimize the damage) even if it is compromised in some hacking attempts. There are many such hashing algorithms which can prove really effective for password security e.g. PBKDF2, bcrypt and scrypt algorithms.

### Never expose information on URLs

Usernames, passwords, session tokens, and API keys should **not** appear in the URL, as this can be captured in web server logs, which makes them easily exploitable.

- `https://api.domain.com/user-management/users/{id}/someAction?apiKey=abcd123456789` ❌❌❌❌❌ **BIG NO**

## References

- <https://docs.microsoft.com/pt-br/azure/architecture/best-practices/api-design>
- <https://blog.mwaysolutions.com/2014/06/05/10-best-practices-for-better-restful-api/>
- <https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9>
- <https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/>
- <https://restfulapi.net/>
