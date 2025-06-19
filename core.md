Here's a breakdown of Django's core components to help you understand how everything fits together:

### 1. **Project vs App**
- **Project**: Your entire website (e.g., `myproject`)
  - Contains settings, main URLs, and connects multiple apps
- **App**: A self-contained module for a specific feature (e.g., `blog`, `users`, `store`)
  - Each app can be reused in different projects

### 2. **Models**
- Define your database structure (like tables in SQL)
- Example:
  ```python
  from django.db import models

  class Product(models.Model):
      name = models.CharField(max_length=100)  # Like VARCHAR(100)
      price = models.DecimalField(max_digits=6, decimal_places=2)
      in_stock = models.BooleanField(default=True)
  ```
- Django automatically:
  - Creates database tables
  - Generates Python methods to query the database

### 3. **Views**
- Handle the logic for what to display to users
- Two types:
  - **Function-based views** (simpler):
    ```python
    def product_list(request):
        products = Product.objects.all()  # Get all products from DB
        return render(request, 'store/product_list.html', {'products': products})
    ```
  - **Class-based views** (more powerful, for complex cases)

### 4. **Templates**
- HTML files with special Django tags for dynamic content
- Example (`product_list.html`):
  ```html
  {% for product in products %}
    <div class="product">
      <h2>{{ product.name }}</h2>
      <p>Price: ${{ product.price }}</p>
    </div>
  {% endfor %}
  ```

### 5. **URLs**
- Maps web addresses to views
- Example (`urls.py`):
  ```python
  from django.urls import path
  from . import views

  urlpatterns = [
      path('products/', views.product_list, name='product-list'),
  ]
  ```

### 6. **Forms**
- Handle user input (both display and validation)
- Example:
  ```python
  from django import forms

  class ContactForm(forms.Form):
      name = forms.CharField(max_length=100)
      email = forms.EmailField()
      message = forms.CharField(widget=forms.Textarea)
  ```
- Can automatically:
  - Generate HTML form fields
  - Validate data
  - Show error messages

### 7. **Admin Interface**
- Automatic admin panel for managing data:
  ```python
  from django.contrib import admin
  from .models import Product

  admin.site.register(Product)
  ```
- Access at `/admin` after creating a superuser

### How They Work Together:
1. User visits `/products/` → URLs file routes to `product_list` view
2. View asks model for data → Model gets data from database
3. View passes data to template → Template renders HTML
4. User sees the product list

### Additional Components:
- **Middleware**: Processes requests/responses globally (like authentication)
- **Static Files**: CSS, JavaScript, images
- **Authentication**: Built-in user system
- **Sessions**: Store temporary user data

Each component handles a specific concern, following Django's "MTV" pattern (Model-Template-View, similar to MVC in other frameworks). This separation makes your code more organized and maintainable as your project grows.