# CSRF Protection in ASP.NET Core

Cross-Site Request Forgery (**CSRF**) is an attack where malicious websites trick authenticated users into submitting unwanted requests to a different website (such as modifying account details or performing unauthorized transactions).

## 🔒 Enabling CSRF Protection in ASP.NET Core
ASP.NET Core provides built-in CSRF protection using **Anti-Forgery Tokens**.

### **1️⃣ Global CSRF Protection Using `AutoValidateAntiforgeryTokenAttribute`**
To enforce CSRF protection on **all state-changing HTTP requests (`POST`, `PUT`, `DELETE`)**, add the following filter in `Program.cs` or `Startup.cs`:

```csharp
services.AddControllersWithViews(options =>
{
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute());
});
```

#### **📌 How It Works**
- Automatically **validates CSRF tokens** for all `POST`, `PUT`, and `DELETE` requests.
- Protects against unauthorized form submissions from other domains.
- Reduces the need to manually apply `[ValidateAntiForgeryToken]` on every controller method.

> 🚀 **Best Practice:** Use this filter globally to ensure all forms and sensitive actions are protected from CSRF attacks.

---

### **2️⃣ Applying CSRF Protection in Individual Controllers**
If you prefer **manual CSRF protection** per action method, use `[ValidateAntiForgeryToken]`:

```csharp
[HttpPost]
[ValidateAntiForgeryToken] // CSRF Protection
public IActionResult UpdateAccount(BankAccount model)
{
    // Process update only if CSRF token is valid
    return View();
}
```

#### **📌 How It Works**
- Ensures that requests include a valid **CSRF token**.
- If the request lacks a valid token, **ASP.NET Core rejects it automatically**.
- Useful for protecting sensitive **form submissions and updates**.

> 💡 **Note:** This method requires explicitly adding `@Html.AntiForgeryToken()` inside your form:

```html
<form method="post">
    @Html.AntiForgeryToken()
    <input type="text" asp-for="AccountNumber" placeholder="Enter your Account Number" />
    <input type="text" asp-for="Pin" placeholder="Enter your PIN" />
    <button class="btn btn-primary" type="submit">Update</button>
</form>
```

---

## 🛡️ Why CSRF Protection is Important
- Prevents **unauthorized transactions** triggered by malicious websites.
- Ensures only **legitimate requests** from authenticated users are processed.
- **Enhances security** for sensitive actions like **updating profiles, changing passwords, or making payments**.

🔗 **Want to Learn More?** Check the [ASP.NET Core Documentation on CSRF](https://learn.microsoft.com/en-us/aspnet/core/security/anti-request-forgery) for detailed insights.

---

### **✅ Summary**
| Approach | Protection Scope | Best Use Case |
|----------|----------------|--------------|
| **Global CSRF Protection** (`AutoValidateAntiforgeryTokenAttribute`) | Applies to **all state-changing HTTP requests** | Recommended for most applications to ensure default security |
| **Manual Protection** (`[ValidateAntiForgeryToken]`) | Applies to **specific controller actions** | Useful when applying CSRF protection selectively |

By implementing **CSRF protection**, your application remains safe from **malicious form submissions** and **unauthorized actions**. 🚀

