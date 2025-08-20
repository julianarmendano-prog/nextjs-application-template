```markdown
# Detailed Implementation Plan for Jugador Libre

This plan outlines the step-by-step modifications and additions needed to build the Jugador Libre platform—a transfer portal for volleyball players, clubs, and coaches in Argentina.

---

## 1. Global Setup and Layout Changes

- **Modify Global CSS**  
  **File:** `src/app/globals.css`  
  - Update typography, spacing, and color palette to reflect a modern, clean design.
  - Add CSS rules for error messages, form field validation states, and responsive layout.
  - Ensure consistency between light/dark themes if applicable.

- **Create/Update Layout Component**  
  **File:** `src/app/layout.tsx`  
  - Wrap all pages with a global layout that imports `globals.css`.
  - Integrate the new Navbar component (see section 2) at the top.
  - Add an optional Footer section for basic platform information.
  - Wrap dynamic content in an error boundary to catch runtime errors.

---

## 2. Create Navbar Component

- **Implement a Modern Navigation Bar**  
  **File:** `src/components/ui/navbar.tsx` (new file)  
  - Build a responsive navbar using semantic HTML and CSS flexbox/grid.
  - Include text-based navigation links: Home, Perfiles, and Auth (Login/Registro).
  - Use plain typography, spacing, and color backgrounds (no external icon libraries).
  - Incorporate mobile-responsive adjustments using the existing hook in `src/hooks/use-mobile.ts`.
  - Add error fallback text if the navigation fails to render.

---

## 3. Develop the Homepage (Landing Page)

- **Design a Hero Section with Call-to-Actions**  
  **File:** `src/app/page.tsx` (create if not present)  
  - Display a welcoming heading “Bienvenido a Jugador Libre” and a descriptive paragraph.
  - Add two prominent buttons: “Registrarse” and “Iniciar Sesión” that navigate to `/auth/register` and `/auth/login`.
  - Insert a hero image using an `<img>` tag with a placeholder URL:  
    `src="https://placehold.co/1920x1080?text=Modern+volleyball+transfer+platform+hero+image"`  
    with a descriptive `alt` text and an `onerror` fallback.
  - Ensure modern spacing, typography, and a clean layout that scales well on mobile.

---

## 4. Build Authentication Pages

- **Login Page**  
  **File:** `src/app/auth/login.tsx` (create new file)  
  - Create a form with fields for email and password.
  - Use components from `src/components/ui/input.tsx` and `src/components/ui/button.tsx` for consistency.
  - Validate fields on the client; display error messages when inputs are invalid.
  - On form submission, POST the data to the API endpoint at `/api/auth/login/route.ts`.

- **Registration Page**  
  **File:** `src/app/auth/register.tsx` (create new file)  
  - Develop a registration form with fields: Full Name, Email, Password, Confirm Password.
  - Include a dropdown (using a custom select from `src/components/ui/select.tsx`) for role selection (Jugador, Entrenador, Club).
  - Validate inputs, ensure password confirmation, and show inline errors.
  - Submit data to `/api/auth/register/route.ts` with proper error handling and success feedback.

---

## 5. Create the User Profile Page

- **Profile Management for Different Roles**  
  **File:** `src/app/profile/page.tsx` (new file)  
  - Develop a form for players to manage their profile details, including:
    - Full Name, Age, Position, Club Name.
    - A toggle switch (using `src/components/ui/switch.tsx`) for “¿Buscar transfer para la próxima temporada?”.
  - For clubs and coaches, adjust the form fields to include relevant details (e.g., club description, coaching specialties).
  - Use components from `src/components/ui/form.tsx`, `input.tsx`, and `label.tsx`.
  - On submission, send a PUT request to `/api/profile/route.ts` with robust client- and server-side validation.

---

## 6. Implement API Endpoints

- **Authentication API Endpoints**  
  - **Login Endpoint**  
    **File:** `src/app/api/auth/login/route.ts`  
    - Implement a POST handler that validates credentials.
    - Use try-catch for error handling and return appropriate HTTP status codes (e.g., 200 for success, 401 for unauthorized).
  
  - **Registration Endpoint**  
    **File:** `src/app/api/auth/register/route.ts`  
    - Create a POST handler to register new users.
    - Validate input data, check for duplicate emails, and handle errors gracefully.
  
- **Profile API Endpoint**  
  **File:** `src/app/api/profile/route.ts`  
  - Implement a PUT handler to update profile information.
  - Validate authentication (session or token simulation) and sanitize input.
  - Return detailed JSON responses with success or error messages.

---

## 7. Optional AI-Powered Match Recommendations

- **AI Endpoint for Transfer Recommendations**  
  **File:** `src/app/api/ai/recommendations/route.ts` (optional new endpoint)  
  - Create a POST endpoint that accepts player profile details.
  - Integrate with an LLM-based API using the provider OpenRouter.
    - **Provider:** OpenRouter  
    - **Model:** Default to `anthropic/claude-sonnet-4`
    - **Endpoint:** `https://openrouter.ai/api/v1/chat/completions`  
  - Format the request using an array of `{ role, content }` objects as per guidelines.
  - Return AI-generated club and coach recommendations, handle errors with fallback messages.

---

## 8. Error Handling & Best Practices

- In every API endpoint, use try-catch blocks and return standardized error objects.  
- Validate all inputs on both client and server sides to avoid security vulnerabilities.  
- Log critical errors using `console.error` and avoid exposing internal details to the client.  
- For UI forms, implement inline validation feedback and disable submit buttons until inputs are valid.

---

## 9. README & Documentation Updates

- **Update README.md**  
  - Document the overall platform purpose, installation steps, and instructions for starting the development server.
  - Include API endpoint routes and expected request/response formats.
  - Provide any necessary notes about environment variables or future integrations (such as the AI service API key).

---

## 10. Testing and Validation

- **End-to-End Testing**  
  - Use curl commands to test API endpoints. For example:
    ```bash
    # Test registration API
    curl -X POST http://localhost:3000/api/auth/register/route.ts \
      -H "Content-Type: application/json" \
      -d '{"fullName": "Juan Perez", "email": "juan@example.com", "password": "secret", "role": "Jugador"}'
    ```
  - Validate proper HTTP status codes and expected JSON responses.
- **UI Testing**  
  - Manually test all responsive views, form validations, navigation links, and error messages.
  - Verify that images load correctly using the specified placeholder URLs and that error fallbacks engage when needed.

---

## Summary

- Created global styling and layout enhancements in `globals.css` and `layout.tsx` for a modern, responsive UI.  
- Introduced a custom Navbar in `src/components/ui/navbar.tsx` featuring text-based links only.  
- Developed landing, authentication (login/register), and profile pages using modern UI components with robust error handling.  
- Added API routes for authentication and profile management with proper validation and try-catch blocks.  
- Optionally integrated an AI-powered recommendation feature using the OpenRouter endpoint and `anthropic/claude-sonnet-4` as default.  
- Updated README.md with comprehensive documentation for installation and API usage.  
- Emphasized best practices in error handling, client-side validations, and responsive design throughout the codebase.
