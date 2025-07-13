## 🎯 1. Main Project (PBL Context)

In BookMart:
- Allow users to **log in with Google**
- Use Spring Security to handle OAuth2 flow
- Access **basic user profile info** from Google
- Store/auto-register users after login (if needed)

---

## 🔍 2. Today’s Problem Statement (PSBL)

**Problem:**  
> We want to **enable Google OAuth2 login** in Spring Boot so that users can authenticate using their Google accounts securely and quickly.

---

## 🎯 3. Learning Objectives

By the end of this session, learners will:

- ✅ Understand the OAuth2 protocol and why it’s used
- ✅ Enable OAuth2 client support in Spring Boot
- ✅ Set up Google Login with client ID & secret
- ✅ Access user profile information from the OAuth2 token
- ✅ (Optional) Auto-register user after first login

---

## 🧠 4. Scenario-Based Framing

> Imagine:
- A user lands on BookMart
- Clicks "Login with Google"
- They authenticate via Google, and we retrieve their profile
- No password management on our side = ✅ secure & scalable

---

## 🗺️ 5. Mini Visual Roadmap

```text
User → Clicks "Login with Google"
↓
Redirects to Google Consent Screen
↓
On Success → Google redirects back with token
↓
Spring Security exchanges token → retrieves user info
↓
App processes user info → optional registration/login flow
````

---

## 📚 6. Conceptual Explanation

### 🔐 What is OAuth2?

OAuth2 is a protocol that allows **secure delegated access** to resources without sharing passwords.

* **User** grants permission to the app
* **App** receives a **token** from a provider (Google, Facebook, GitHub)
* The token is used to **fetch user info**

---

### 🔑 Why Use OAuth2?

| Benefit                | Description                          |
| ---------------------- | ------------------------------------ |
| No password management | Google handles authentication        |
| Secure & scalable      | Uses industry-standard protocol      |
| Simplifies UX          | Single-click login for users         |
| Integrates with Spring | Spring Security OAuth2 makes it easy |

---

## 💻 7. Hands-On Implementation

---

### ✅ Step 1: Create Google OAuth2 Credentials

* Go to [https://console.cloud.google.com](https://console.cloud.google.com)
* Create a project
* Go to **OAuth2 consent screen** → Configure
* Create **OAuth Client ID**

  * App type: Web
  * Redirect URI: `http://localhost:8080/login/oauth2/code/google`
* Copy the **Client ID** and **Client Secret**

---

### ✅ Step 2: Add Spring Security OAuth2 Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

---

### ✅ Step 3: Configure `application.properties`

```properties
spring.security.oauth2.client.registration.google.client-id=YOUR_GOOGLE_CLIENT_ID
spring.security.oauth2.client.registration.google.client-secret=YOUR_GOOGLE_CLIENT_SECRET
spring.security.oauth2.client.registration.google.scope=email,profile
spring.security.oauth2.client.registration.google.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}
spring.security.oauth2.client.provider.google.authorization-uri=https://accounts.google.com/o/oauth2/v2/auth
spring.security.oauth2.client.provider.google.token-uri=https://oauth2.googleapis.com/token
spring.security.oauth2.client.provider.google.user-info-uri=https://www.googleapis.com/oauth2/v3/userinfo
```

---

### ✅ Step 4: Enable OAuth2 Login in Spring Security

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/login", "/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2Login(); // Enables Google Login
        return http.build();
    }
}
```

---

### ✅ Step 5: Handle OAuth2 User Info

You can create a custom user service:

```java
@Service
public class CustomOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {

    @Override
    public OAuth2User loadUser(OAuth2UserRequest userRequest) {
        OAuth2User user = new DefaultOAuth2UserService().loadUser(userRequest);
        Map<String, Object> attributes = user.getAttributes();

        String email = (String) attributes.get("email");
        String name = (String) attributes.get("name");

        // Optionally save user to DB or create local session

        return user;
    }
}
```

Inject this into the security config:

```java
.oauth2Login(oauth2 -> oauth2.userInfoEndpoint()
    .userService(customOAuth2UserService))
```

---

## 📤 8. Output-Based Assessment

| ✅ Task                               | 💬 Expected Outcome                         |
| ------------------------------------ | ------------------------------------------- |
| Google login screen appears          | Redirects to Google consent screen          |
| Token returned                       | App receives and validates token            |
| User info printed or stored          | Name, email, and profile picture retrieved  |
| Login triggers authenticated session | User can access protected pages after login |
| GitHub push                          | `feature/oauth2-google-login` branch pushed |

---

## 🎯 9. Interview Preparation

### Q1. What is OAuth2 and how is it different from traditional login?

> OAuth2 lets users authenticate using third-party providers without sharing passwords directly with your app.

### Q2. How does Spring Boot simplify OAuth2 login?

> It auto-configures login endpoints, redirects, token handling, and user info fetch using just properties.

### Q3. What info can you get from the Google token?

> Name, email, picture, locale, sub (user ID), etc.

### Q4. How do you store or track logged-in users?

> Use a custom `OAuth2UserService` to store user info in the local database if not already present.

---

## 🔄 10. Connection to the Next Problem Statement

OAuth2 login is now working.
In the next session, we’ll focus on:

* **Writing integration tests** for secured endpoints
* Mocking authentication for `@PreAuthorize` testing

---

## ✅ Next Topic:

### Session 24 → Testing OAuth2 + Secure APIs using JUnit & Spring Security Test

