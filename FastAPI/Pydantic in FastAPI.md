### Pydantic Models: Validation and Serialization of Data

Pydantic is a powerful library used for data validation and settings management. It integrates seamlessly with FastAPI to validate and serialize data. It’s also used for handling input validation when creating API endpoints. Here’s how to work with Pydantic models in FastAPI:

#### 1. **Pydantic Model Structure**

A Pydantic model is defined by creating a class that inherits from `pydantic.BaseModel`. Pydantic then automatically validates the fields when an object is created.

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str
    price: float
    tax: float = None  # Default value can be provided

# Example usage
item = Item(name="Laptop", description="A high-end laptop", price=1200.00)
print(item.dict())
```

In this example, `Item` is a Pydantic model, and its fields include `name`, `description`, `price`, and an optional `tax` field. Pydantic will validate the types automatically.

#### 2. **Type Hints and Automatic Validation**

FastAPI automatically reads type hints provided in function signatures and uses them for input data validation. This means that FastAPI knows how to validate incoming request data based on the type hints in your endpoint's function.

Here’s an example of how to use type hints in FastAPI:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str
    price: float
    tax: float = None

@app.post("/items/")
async def create_item(item: Item):
    return {"item_name": item.name, "item_price": item.price}
```

FastAPI will automatically ensure that the data passed in the POST request matches the type annotations in the `Item` model. If the data doesn’t match (e.g., if a field is missing or the type is incorrect), FastAPI will return a 422 Unprocessable Entity status code with an error message.

#### 3. **Custom Validation**

You can also add custom validation to a Pydantic model by using the `@root_validator` decorator or custom field validators.

##### Example of Custom Field Validation

If you need a custom validation rule, such as ensuring the `price` is greater than 0, you can add a validator to the Pydantic model.

```python
from pydantic import BaseModel, validator

class Item(BaseModel):
    name: str
    description: str
    price: float
    tax: float = None

    @validator('price')
    def check_price(cls, value):
        if value <= 0:
            raise ValueError("Price must be greater than zero")
        return value
```

In this case, the `@validator` decorator is used to define a custom validation function on the `price` field. If the price is less than or equal to 0, a `ValueError` is raised.

##### Example of `@root_validator`

The `@root_validator` is useful when you need to validate multiple fields together. For example, you might want to ensure that the `price` and `tax` are consistent, or that the total price is positive.

```python
from pydantic import BaseModel, root_validator

class Item(BaseModel):
    name: str
    description: str
    price: float
    tax: float = None

    @root_validator
    def check_total_price(cls, values):
        price = values.get('price')
        tax = values.get('tax', 0)
        if price + tax < 0:
            raise ValueError("Total price (price + tax) cannot be negative")
        return values
```

Here, the `@root_validator` checks both the `price` and `tax` fields at the same time to ensure that their sum is non-negative.

### Conclusion

Using **Pydantic** with **FastAPI** makes validation and serialization of data straightforward. With **type hints**, you can let FastAPI handle validation automatically, and with **custom validation** methods like `@validator` and `@root_validator`, you can enforce additional rules that are unique to your application.