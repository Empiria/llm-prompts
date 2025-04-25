```markdown
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

**Avoid in Forms**:
- Business rule enforcement
- Database operations
- Complex calculations
- Application state management

```python
# Good Form Example
class RegistrationForm(BaseForm):
    def __init__(self, **properties):
        self.init_components(**properties)

    def register_button_click(self, **event_args):
        try:
            # Pass data to server module
            anvil.server.call('register_user', 
                self.email_input.text,
                self.password_input.text
            )
            self.status_lbl.text = "Registration successful!"
            # Navigate to another page after successful registration
            self.navigate('/')
        except Exception as e:
            self.status_lbl.text = f"Error: {str(e)}"
```

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
    UserTable.add(email=email, 
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
```

Key benefits:
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

    def navigate(self, route):
        self.router.navigate(route)
```

```python
# Usage in a form
class ProductForm(BaseForm):
    def view_product_click(self, **event_args):
        self.navigate(f'/products/{self.product.id}')
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
# Form
def update_click(self, **event_args):
    try:
        anvil.server.call('add_user', self.name_input.text)
    except ValidationError as e:
        self.error_label.text = str(e)

# Server Module
@anvil.server.callable 
def add_user(name):
    validate_name(name)  # Raises ValidationError
    User.create(name)
    check_user_capacity()  # Server-side check
```

✅ **Best Practice: Model Class**
```python
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
    def update_click(self, **event_args):
        try:
            User.create(self.name_input.text)
            self.status_lbl.text = "User created successfully"
            self.navigate('/users')
        except (ValidationError, BusinessError) as e:
            self.error_label.text = str(e)
```
```
