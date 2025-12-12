# BlazorApp1 - Project Documentation

## Project Overview

BlazorApp1 is a modern ASP.NET Core 8.0 Blazor Web Application using the Interactive Server rendering mode. The application features a responsive UI built with MudBlazor components and includes comprehensive authentication and authorization capabilities integrated with Auth0.

### Technology Stack

- **.NET 8.0** - Latest .NET framework with Blazor Web App
- **Blazor Interactive Server** - Server-side rendering with interactivity
- **MudBlazor 8.14.0** - Modern Material Design UI component library
- **ASP.NET Core Authentication** - Cookie + OpenID Connect integration
- **Auth0** - External authentication provider

### Project Structure

```
BlazorApp1/

# BlazorApp1 - Project Documentation

## Project Overview

BlazorApp1 is a modern ASP.NET Core 8.0 Blazor Web Application using Interactive Server rendering mode. The application features a responsive UI built with MudBlazor components and includes comprehensive authentication and authorization capabilities integrated with Auth0.

### Technology Stack

- **.NET 8.0** - Latest .NET framework with Blazor Web App
- **Blazor Interactive Server** - Server-side rendering with interactivity
- **MudBlazor 8.14.0** - Modern Material Design UI component library
- **ASP.NET Core Authentication** - Cookie + OpenID Connect integration
- **Auth0** - External authentication provider

### Key Features

- Responsive Material Design UI with MudBlazor
- Server-side Blazor with interactive components
- Navigation drawer with organized menu structure
- Authentication and authorization with Auth0
- Role-based and permission-based access control
- Protected routes and pages
- Reusable authorization components

---

## Auth0 Integration

### Overview

The application implements comprehensive Auth0 authentication using ASP.NET Core's built-in authentication middleware. This integration provides secure user authentication, single sign-on capabilities, and fine-grained authorization control.

### Architecture

- **Cookie Authentication**: Local session management
- **OpenID Connect**: Secure integration with Auth0
- **Authorization Services**: Role and permission-based access control
- **Cascading Authentication State**: Blazor authentication context

### Configuration

#### appsettings.json
```json
{
  "Auth0": {
    "Domain": "YOUR_AUTH0_DOMAIN",
    "ClientId": "YOUR_AUTH0_CLIENT_ID", 
    "ClientSecret": "YOUR_AUTH0_CLIENT_SECRET"
  }
}
```

#### Program.cs Setup
```csharp
// Authentication services
builder.Services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
})
.AddCookie()
.AddOpenIdConnect(options =>
{
    options.Authority = $"https://{builder.Configuration["Auth0:Domain"]}";
    options.ClientId = builder.Configuration["Auth0:ClientId"];
    options.ClientSecret = builder.Configuration["Auth0:ClientSecret"];
    options.ResponseType = "code";
    
    // Scopes
    options.Scope.Clear();
    options.Scope.Add("openid");
    options.Scope.Add("profile");
    options.Scope.Add("email");
    
    // Callback paths
    options.CallbackPath = "/callback";
    options.SignedOutCallbackPath = "/signout-callback";
    options.RemoteSignOutPath = "/signout-oidc";
    
    // Token validation
    options.TokenValidationParameters.NameClaimType = "name";
    options.TokenValidationParameters.RoleClaimType = "https://schemas.auth0.com/authorization/roles";
});
```

### Authentication Endpoints

#### Login Endpoint
- **URL**: `/login`
- **Method**: GET
- **Function**: Redirects to Auth0 for authentication
- **Return URL**: Preserves original destination after login

#### Logout Endpoint
- **URL**: `/logout`
- **Method**: GET
- **Function**: Clears local session and Auth0 session
- **Redirect**: Returns to home page

### UI Integration

#### Login/Logout Buttons
Located in [MainLayout.razor](cci:7://file:///c:/Users/theo/RiderProjects/BlazorApp1/BlazorApp1/Components/Layout/MainLayout.razor:0:0-0:0) app bar:
``` .