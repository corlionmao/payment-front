/create-agent Act as a Principal Frontend Engineer specializing in Angular 17+ and robust enterprise state architectures. 

Generate a complete, production-ready Angular SPA client designed to interact with the previously implemented .NET Core Clean Architecture Web API endpoint. The entire codebase must utilize Standalone Components, strict functional features, Angular Signals for performance optimization, and provide elegant error boundary interception.

### Workspace Architecture & Project Layout
Generate all files using a Feature-Based design structure with the following organization:
- src/app/core/guards: Contains functional route authorization filters ('auth.guard.ts').
- src/app/core/interceptors: Contains global token mapping injectors ('auth.interceptor.ts').
- src/app/core/services: Holds central system session stores ('auth.service.ts').
- src/app/features/auth: Contains component layouts for application access validation ('login.component.ts' + template HTML).
- src/app/features/payments/models: Declares immutable contract interface mappings mirroring .NET objects ('payment.model.ts').
- src/app/features/payments/services: Integrates with backend endpoints using RxJS Observables ('payment.service.ts').
- src/app/features/payments/components: Houses standalone UI components for ledger management ('payment-list.component.ts', 'payment-form.component.ts' and their template files).

### Contract Interface Mappings (payment.model.ts)
Implement structural TypeScript interfaces representing transaction payloads:
- 'CreatePaymentCommand': Properties for correlationId (string), amount (number), currency ('USD' | 'EUR' | 'GBP'), and accountId (string).
- 'PaymentResponse': Fields for paymentId (string) and status (string).
- 'PaymentDetailsDto': Structures for id (string), amount (number), currency (string), status ('Pending' | 'Completed' | 'Failed'), createdAt (string), and failureReason (string | null).

### Global State Infrastructure & Authentication Services (Core Layer)
- 'AuthService': Exposes a Signal or 'BehaviorSubject<boolean>' tracking active sessions. Build a 'login(credentials)' method that hits POST 'http://localhost:5000/api/auth/login'. On success, cache the JWT token string in localStorage. Implement 'getToken()' and a clear-state 'logout()' routing hook. Use catchError to intercept backend RFC 7807 ProblemDetails responses and transform them into human-readable Error instances.
- 'authInterceptor' (Functional Interceptor): Intercepts outbound HTTP streams. If an active JWT string resides in storage, it clones the request record and appends the 'Authorization: Bearer <token>' header key.
- 'appConfig' ('app.config.ts'): Registers 'provideRouter', 'provideHttpClient', and explicitly attaches 'withInterceptors([authInterceptor])' to the network pool pipeline.

### Payment CRUD Services (Feature Layer)
- 'PaymentService': Directs network calls matching the backend controller. Implement methods:
  * 'getPayments()': GET request returning 'PaymentDetailsDto[]'.
  * 'createPayment(command)': POST request to push a new entry.
  * 'completePayment(id)': PUT request targeting '/api/payments/{id}/complete' to invoke status alterations (Update CRUD requirement).
  * 'cancelPayment(id)': DELETE request targeting '/api/payments/{id}' to trigger audited soft deletes (Delete CRUD requirement).

### UI Presenters, Signal State Manipulation & Performance (Feature Components)
1. 'LoginComponent': Features a template-driven or Reactive Form ('FormGroup') capturing username and password fields. Handles server-side issues gracefully through local error-boundary text strings. On completion, routes to the dashboard ledger.
2. 'PaymentListComponent' (The Dashboard Ledger Grid): 
   * Performance Win: Configured strictly to use 'ChangeDetectionStrategy.OnPush'.
   * State Win: Manages internal arrays via fine-grained Angular Signals ('public payments = signal<PaymentDetailsDto[]>([])').
   * Template: Uses modern control flow tags ('@for (payment of payments(); track payment.id)') ensuring optimal DOM node redraw efficiency. Displays an elegant HTML grid tracking ID, Amount, Currency, and Status with semantic badges. In addition, attach two rows of action buttons: an interactive "✔️ Complete" trigger that maps to update operations and a "🗑️ Cancel" component to run soft deletes.
3. 'PaymentFormComponent' (Idempotency Creator): Generates a secure client-side cryptographic string ('crypto.randomUUID()') immediately inside 'ngOnInit' and maps it to a read-only token form field. Employs form fields for Amount, Currency dropdown selections, and Account references. Upon successful submission, returns focus to the transaction ledger.

### Secure Global Router Configurations ('app.routes.ts')
Configure lazy-loading code division structures on routes. Wrap structural access inside an 'authGuard' function that diverts unauthorized sessions back to the '/login' route. Set up the schema path boundaries cleanly so '/payments' manages nested child rendering paths for lists and creations.

Provide all components fully coded with zero placeholders or structural comments. Append a comprehensive CSS style template matching premium enterprise visual styles.
