# Seventh-challenge
This project implements an interactive menu system in Python, allowing users to manage a custom order with classes for beverages, appetizers, and main courses. Users can add or remove items from the order, calculate the total bill, and receive an automatic discount if the order contains more than three items. Additionally, the `Order` class includes a custom iterator that allows for easy iteration over the items in the order, displaying details such as the name, quantity, and price of each item, providing a simple and organized experience.
```python
class MenuItem:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def calculate_total_price(self, quantity=1):
        return self.price * quantity

class Beverage(MenuItem):
    def __init__(self, name, price, size):
        super().__init__(name, price)
        self.size = size

class Appetizer(MenuItem):
    def __init__(self, name, price, servings):
        super().__init__(name, price)
        self.servings = servings

class MainCourse(MenuItem):
    def __init__(self, name, price, ingredients):
        super().__init__(name, price)
        self.ingredients = ingredients

class OrderIterator:
    def __init__(self, items):
        self._items = list(items.values())
        self._index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self._index < len(self._items):
            item_info = self._items[self._index]
            self._index += 1
            return item_info["item"], item_info["quantity"]
        else:
            raise StopIteration

class Order:
    def __init__(self):
        self.items = {}

    def add_item(self, item, quantity=1):
        if item.name in self.items:
            self.items[item.name]["quantity"] += quantity
        else:
            self.items[item.name] = {"item": item, "quantity": quantity}

    def remove_item(self, item_name, quantity=1):
        if item_name in self.items:
            if quantity >= self.items[item_name]["quantity"]:
                del self.items[item_name]
            else:
                self.items[item_name]["quantity"] -= quantity
        else:
            print(f"'{item_name}' no está en el pedido.")

    def calculate_total_bill(self):
        total_bill = 0
        for item_info in self.items.values():
            item = item_info["item"]
            quantity = item_info["quantity"]
            total_bill += item.calculate_total_price(quantity)

        total_bill -= self.calculate_discount()

        return total_bill
    
    def calculate_discount(self):
        if len(self.items) > 3:
            return self.calculate_total_bill() * 0.05
        else:
            return 0

    def __iter__(self):
        return OrderIterator(self.items)

if __name__ == "__main__":
    beverage1 = Beverage("Coffee", 1.50, "Small")
    beverage2 = Beverage("Tea", 1.50, "Small")
    beverage3 = Beverage("Soft drinks", 2.00, "Small")
    beverage4 = Beverage("Fruit juice", 2.50, "Small")
    beverage5 = Beverage("Water", 1.00, "Small")
    beverage6 = Beverage("Sparkling water", 1.50, "Small")

    appetizer1 = Appetizer("Potato soup Colombian style", 6.99, 1)
    appetizer2 = Appetizer("Fried squid with spicy tomato sauce", 9.99, 1)

    main_course1 = MainCourse("Brazilian Steak", 24.99, ["Beef", "Potatoes", "Vegetables"])
    main_course2 = MainCourse("Grilled Fish", 18.50, ["Fish", "Rice", "Vegetables"])
    main_course3 = MainCourse("Roast Chicken", 16.99, ["Chicken", "Mashed Potatoes", "Green Beans"])

    order = Order()
    order.add_item(beverage1, 2)
    order.add_item(appetizer1, 1)
    order.add_item(main_course1, 1)

    for item, quantity in order:
        print(f"{item.name} x{quantity}: ${item.calculate_total_price(quantity):.2f}")
        

```
