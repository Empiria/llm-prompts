<!--
SPDX-License-Identifier: MIT
Copyright (c) Empiria Ltd
This file is part of the LLM Prompts project, published at https://github.com/empiria/llm-prompts
-->

# Anvil Framework Application Structure Guide

## Objective  
Create a well-structured Anvil web application that enforces **separation of concerns** between  
UI components, business logic, and data management. Focus on maintainability and testability  
through modular design.

---

## Key Principles

### 1. **Form Responsibilities**  
Forms should **only** handle:
- UI rendering and updates
- User input collection
- Event handling for user interactions
- Navigation using the routing module
- Validation feedback (presentation layer only)
- **Utilize data bindings** for UI components to model attributes or form variables

**Avoid in Forms**:
- Business rule enforcement
- Database operations
- Complex calculations
- Application state management

```python
# Good Form Example with Data Binding
from anvil.routing import navigate

class RegistrationForm(BaseForm):
    def __init__(self, **properties):
        self.init_components(**properties)
        # Bind UI components to form-level variables
        self.user_email = ""
        self.status_message = ""
        self.refresh_data_bindings()
    
    def register_button_click(self, **event_args):
        try:
            # Pass data to server module
            anvil.server.call('register_user', 
                self.user_email,
                self.user_password
            )
            self.status_message = "Registration successful!"
            self.refresh_data_bindings()
            # Navigate to another page after successful registration
            navigate('/')
        except Exception as e:
            self.status_message = f"Error: {str(e)}"
            self.refresh_data_bindings()
```

**Explanation:**
- **Data Binding:** UI components such as `email_input.text` and `status_lbl.text` are bound to `self.user_email` and `self.status_message`, respectively.
- **self.refresh_data_bindings():** Called after updating bound variables to ensure the UI reflects the latest state.
- **Navigation:** Use the `navigate` function from the routing module to change pages.

### 2. **Server Modules**  
Handle:
- Business logic
- Data validation
- Database operations
- External API calls
- Security checks

```python
# Server module example
@anvil.server.callable
def register_user(email, password):
    # Business logic validation
    if not validate_email_format(email):
        raise ValueError("Invalid email format")
    
    if UserTable.get(email=email):
        raise ValueError("User already exists")
    
    # Data handling
    User.create(email=email, 
                password_hash=hash_password(password))
```

### 3. **Data Layer**  
- Use Anvil Data Tables for simple storage requirements.
- Utilize Anvil's Model Classes to create clean, object-oriented interfaces to your Data Tables:

```python
# Model class example
@anvil.server.portable_class
class User(Model):
    __table__ = app_tables.users
    
    # Property demonstrating computed/derived data
    @property
    def full_name(self) -> str:
        return f"{self.first_name} {self.last_name}"
    
    # Class method for finding users
    @classmethod
    def get_active_users(cls):
        return cls.search(status="active")
    
    # Instance method for user-specific operations
    def deactivate(self):
        self.update(status="inactive",
                   deactivated_at=datetime.now())
    
    @classmethod
    def create(cls, email: str, password_hash: str) -> "User":
        if not email or not password_hash:
            raise ValidationError("Email and password are required.")
        return cls.add_row(email=email, password_hash=password_hash, status="active")
```

**Key benefits:**
- Cleaner, more Pythonic data access
- Type hints and code completion in your IDE
- Business logic contained within the relevant data
- Portable between client and server code

### 4. **Service Layer (Optional)**
- External integrations
- Background tasks
- Complex business workflows

```python
# Service example (server module)
@anvil.server.background_task
def process_order_async(order_id):
    order = Order.get(order_id)
    PaymentService.process(order)
    InventoryService.update_stock(order)
```

---

## Structural Guidelines

1. **Project Organization**  
    ```
    /myapp
      /client_code
        /forms
          /components
          /layouts
          /pages
        /services
          /formatters
          /model
          /routes
      /server_code
      anvil.yaml
    ```

2. **Routing**
- Use the routing module for clean URLs and navigation
- Define routes in a central location
- Keep route handlers lightweight

```python
# /client_code/services/routes/routes.py
from anvil.routing import Router, routing

router = Router()

@routing.route('/', title='Home')
def home_page():
    return HomeForm()

@routing.route('/users/:user_id', title='User Profile')
def user_profile(user_id):
    user = User.get(user_id)
    return UserProfileForm(user=user)

@routing.route('/products', title='Products')
def products(category=None, sort=None):
    """Example with query parameters: /products?category=books&sort=price"""
    return ProductsForm(category=category, sort=sort)
```

```python
# /client_code/forms/pages/base_form.py
from anvil.routing import Router

class BaseForm(Form):
    def __init__(self, **properties):
        self.router = Router()
        self.init_components(**properties)
```

```python
# Usage in a form
from anvil.routing import navigate

class ProductForm(BaseForm):
    def view_product_click(self, **event_args):
        navigate(f'/products/{self.product.id}')
```

3. **State Management**  
- Use Anvil's session storage sparingly
- Prefer parameter passing between forms
- Store sensitive data server-side

4. **Error Handling**  
- Handle UI-friendly errors in forms
- Log technical errors server-side
- Use custom exception classes for business errors

---

## Anti-Patterns to Avoid

❌ **Form Monolith**  
```python
# Bad: Business logic in form
def update_click(self, **event_args):
    # Data validation
    if len(self.name_input.text) < 3:
        raise ValueError("Name too short")
    
    # Database operation
    app_tables.users.add_row(name=self.name_input.text)
    
    # Business logic
    if user_count() > MAX_USERS:
        send_admin_alert()
```

✅ **A Better Approach**  
```python
from anvil.routing import navigate

# Form
class UserForm(BaseForm):
    def __init__(self, **properties):
        self.init_components(**properties)
        self.user_name = ""
        self.status_message = ""
        self.refresh_data_bindings()
    
    def update_click(self, **event_args):
        try:
            anvil.server.call('add_user', self.user_name)
            self.status_message = "User added successfully."
            self.refresh_data_bindings()
            navigate('/users')
        except ValidationError as e:
            self.status_message = str(e)
            self.refresh_data_bindings()
```

```python
# Server Module
@anvil.server.callable 
def add_user(name):
    validate_name(name)  # Raises ValidationError
    User.create(name=name)
    check_user_capacity()  # Server-side check
```

✅ **Best Practice: Model Class**
```python
from anvil.routing import navigate

# Model class with business logic
@anvil.server.portable_class
class User(Model):
    __table__ = app_tables.users
    
    @classmethod
    def create(cls, name: str) -> "User":
        if len(name) < 3:
            raise ValidationError("Name too short")
            
        if cls.count() >= MAX_USERS:
            alert_admin("User capacity reached")
            raise BusinessError("Cannot create more users")
            
        return cls.add_row(name=name)

# Form
class UserForm(BaseForm):
    def __init__(self, **properties):
        self.init_components(**properties)
        self.user_name = ""
        self.status_message = ""
        self.refresh_data_bindings()
    
    def update_click(self, **event_args):
        try:
            User.create(self.user_name)
            self.status_message = "User created successfully"
            self.refresh_data_bindings()
            navigate('/users')
        except (ValidationError, BusinessError) as e:
            self.status_message = str(e)
            self.refresh_data_bindings()
```

**Explanation:**
- **Data Binding:** The form binds `self.user_name` and `self.status_message` to UI components, reducing direct manipulation.
- **self.refresh_data_bindings():** Ensures that any changes to bound variables are reflected in the UI.
- **Navigation:** Use the `navigate` function for page transitions.
- **Model Class Logic:** Business logic is encapsulated within the `User` model, keeping the form clean and focused on UI interactions.
