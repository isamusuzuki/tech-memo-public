# Controller で ログイン済みの UserDetails を取得する

作成日 2025/03/11

参照サイト => [Get UserDetails object from Security Context in Spring MVC controller](https://stackoverflow.com/questions/6161985/get-userdetails-object-from-security-context-in-spring-mvc-controller)

## 1. Spring Security を利用した、堅牢バージョン

```java
package com.example.demo.controllers;

import org.springframework.security.authentication.AnonymousAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class RootController {

    @GetMapping("/authenticated")
    public String authenticated() {
        StringBuilder sb = new StringBuilder();
        sb.append("<h1>authenticated page</h1>");

        Authentication auth = SecurityContextHolder.getContext().getAuthentication();
        if(!(auth instanceof AnonymousAuthenticationToken)) {
            UserDetails userDetails = (UserDetails) auth.getPrincipal();
            String username = userDetails.getUsername();
            sb.append("<p>username: " + username + "</p>");
        }

        return sb.toString();
    }
}
```

## 2. Spring Security を省略した、お手軽バージョン

```java
package com.example.demo.controllers;

import java.security.Principal;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import jakarta.servlet.http.HttpServletRequest;

@RestController
public class RootController {

    @GetMapping("/authenticated")
    public String authenticated(HttpServletRequest request) {
        StringBuilder sb = new StringBuilder();
        sb.append("<h1>fileout authenticated page</h1>");

        String username = null;
        Principal principal = request.getUserPrincipal();
        if (principal != null) {
            username = principal.getName();
            sb.append("<p>username: " + username + "</p>");
        }

        return sb.toString();
    }
}
```
