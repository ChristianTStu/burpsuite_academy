# Path Traversal Vulnerability

## Definition

Path traversal (or directory traversal) vulnerabilities allow attackers to access arbitrary files on a server. This can expose sensitive data, application files, or system credentials. If an attacker can also write to these files, they may modify application behavior or gain full control of the server.

## Affected Files / Components

1. Application code and data
2. Credentials for back-end systems
3. Sensitive operating system files

--- 
## Exploitation Example

### Scenario

A shopping app loads images using URLs like:

```
<img src="/loadImage?filename=218.png">
```

The `loadImage` endpoint retrieves files from `/var/www/images/` by appending the requested filename.

### Code / URL Example

```
/var/www/images/218.png
```

#### Exploit Example

An attacker can request:

```
https://insecure-website.com/loadImage?filename=../../../etc/passwd
```

This resolves to:

```
/var/www/images/../../../etc/passwd → /etc/passwd
```

On Windows, both `../` and `..\` work similarly:

```
https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini
```

This allows attackers to access sensitive files on the server.

---
## Lab: File Path Traversal (Simple Case)

**Lab URL:**  
[Web Security Academy Lab](https://0af20004033370f8801cda2600c40021.web-security-academy.net/)  
_Must be opened in PortSwigger Browser within Burp Suite_

### Steps to Solve:

4. Open **Burp Suite**
5. Go to **Proxy** tab
6. Load the website using **PortSwigger Browser**
7. Open **HTTP History** in the Proxy tab
8. Find requests to `loadimage?filename=` (disable all filters in Burp to view these requests)
9. Select a request and send it to **Repeater**
10. Modify the file path in the **Repeater** following the example above
11. Solution:
    
    ```
    GET /image?filename=/../../../etc/passwd
    ```

---
## Additional Notes

- Defenses against path traversal include input validation, using allowlists for file paths, and restricting access to sensitive directories.