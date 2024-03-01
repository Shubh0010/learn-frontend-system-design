# Security Headers


## X-Powered-By

  - Objective: Conceal server details to mitigate potential attack vectors.
  - Implementation: Remove the X-Powered-By header from responses to avoid disclosing server information to potential attackers.

  ```
  app.use((req, res, next) => {
    res.removeHeader('X-Powered-By');
    next();
  });
  ```

## Referrer-Policy

  - Objective: Protect sensitive information transmitted via URLs during cross-domain redirects.
  - Implementation: Set a Referrer-Policy header to specify how much referrer information should be included in the request.

## X-Content-Type-Options

  - Background: You're developing a web application where users can upload images. Your application serves these images to other users.
  - Challenge: You want to ensure that the browser strictly respects the declared content type of the images and does not attempt to "sniff" the content to determine its type. This is crucial because if a malicious user uploads a file that appears to be an image but contains malicious JavaScript, browsers might incorrectly interpret it as HTML and execute the script.
  - Solution: By setting the X-Content-Type-Options header to 'nosniff', you instruct the browser not to perform MIME-sniffing and to strictly adhere to the declared content type provided by the server.
  - Example Implementation: When a user uploads an image, your server sets the Content-Type header to 'image/jpeg' or 'image/png' based on the file's actual type. Additionally, your server sends the X-Content-Type-Options header with the value 'nosniff' to instruct the browser not to override the declared content type.
  - Outcome: With the X-Content-Type-Options header in place, your web application protects against MIME-sniffing attacks. Browsers will not attempt to guess the content type of uploaded images, mitigating the risk of executing malicious scripts disguised as images. This enhances the security of your application and ensures a safer browsing experience for your users.

  ```
  app.use((req, res, next) => {
    res.setHeader('X-Content-Type-Options', 'nosniff');
    next();
  });
  ```

## X-XSS-Protection

- Objective: Defend against cross-site scripting (XSS) attacks by enabling built-in browser protections.

## HSTS (HTTP Strict Transport Security Header)

- You need to safeguard against attacks like protocol downgrade, where an attacker intercepts and downgrades a user's connection from HTTPS to HTTP, making it vulnerable to eavesdropping and tampering.
- Implement HTTP Strict Transport Security (HSTS) to enforce secure communication over HTTPS. With HSTS, you instruct the browser to always use HTTPS when communicating with your website, reducing the risk of protocol downgrade attacks.
- Configure your web server to send the Strict-Transport-Security header with a specified max-age directive, indicating how long (in seconds) the browser should remember to enforce HTTPS. Additionally, include the 'includeSubDomains' directive to apply the policy to all subdomains and consider preloading the HSTS policy to enhance security.

```
res.setHeader('Strict-Transport-Security', 'max-age=10000; includeSubDomains; preload');
```

- Despite implementing HTTP Strict Transport Security (HSTS) to enforce HTTPS connections, you want to further enhance security by ensuring that all initial connections to your domain are automatically redirected to HTTPS, without the initial HTTP request.
- Register your domain with hstspreload.org and follow their guidelines to add your domain to the HSTS preload list. This action instructs browsers to always use HTTPS when accessing your domain, eliminating the need for an initial HTTP request and providing immediate security benefits for all users.