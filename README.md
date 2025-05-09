from datetime import datetime, timedelta

# Product data: name → (shelf life in days, production time in minutes)
product_data = {
    "pandesal": (2, 10),
    "donut": (2, 15),
    "monay": (3, 12),
    "tasty": (4, 20),
    "kababayan": (3, 15),
    "ensaymada": (4, 25),
    "spanish bread": (3, 18),
    "kalihim": (3, 15),
    "brownies": (5, 30),
    "biscuits": (7, 20),
    "crinkles": (5, 25),
    "hopia": (7, 30),
    "cupcake": (4, 20),
    "cake": (5, 40),
    "muffin": (4, 20),
    "banana bread": (5, 35),
    "croissant": (2, 25),
    "cookies": (4, 22),
    "buko pies": (6, 28),
    "bread rolls": (3, 18),
    "cheesecake": (5, 35),
    "tarts": (4, 30),
    "pretzels": (3, 20),
    "loaf": (4, 25),
    "asado siopao": (6, 18),
    "pandecoco": (3, 12),
    "putok": (3, 10),
    "empanada": (5, 20),
    "cheese bread": (4, 22),
    "garlic bread": (3, 20),
    "ube cheese pandesal": (4, 18),
    "coffee": (2, 15),
    "milk": (1, 12),
    "reno": (1, 10),
    "star margarine": (2, 8),
    "peanut butter": (3, 18),
    "mayonnaise": (2, 12)
}

# Order class with tracking
class Order:
    def __init__(self, name):
        self.name = name
        self.shelf_life_days, self.production_time = product_data[name]
        self.date_ordered = datetime.now()
        self.expiration_time = self.date_ordered + timedelta(days=self.shelf_life_days)
        self.production_start_time = None
        self.production_end_time = None

    def produce(self, current_time):
        self.production_start_time = current_time
        self.production_end_time = current_time + timedelta(minutes=self.production_time)
        return self.production_end_time <= self.expiration_time

    def __str__(self):
        return f"{self.name.title()} - Expires: {self.expiration_time.strftime('%Y-%m-%d')}"

# Step 1: Show available products
print("📋 Available Bakery Products and Shelf Life (days):\n")
for name, (shelf_life, _) in product_data.items():
    print(f"• {name.title():15} - {shelf_life} day(s)")

# Step 2: Collect orders from user
orders = []
print("\n🧾 Good Day! Enter your orders (type 'done' when finished):\n")
while True:
    order = input("Enter product name: ").strip().lower()
    if order == "done":
        break
    elif order in product_data:
        new_order = Order(order)
        orders.append(new_order)
        print(f"✅ '{order.title()}' added to queue (expires on {new_order.expiration_time.strftime('%Y-%m-%d')}).\n")
    else:
        print("❌ Invalid product. Please try again.\n")

# Step 3: Sort orders by expiration date
orders.sort(key=lambda o: o.expiration_time)

# Step 4: Show the queued orders
print("\n📦 Order Queue (by expiration date):\n")
for i, order in enumerate(orders, 1):
    print(f"{i}. {order}")

# Step 5: Simulate production and check deadlines
print("\n⚙️  Sending Orders to Production...\n")
current_time = datetime.now()

for i, order in enumerate(orders, 1):
    on_time = order.produce(current_time)
    status = "✅ On time" if on_time else "❌ Missed deadline"
    print(f"{i}. {order.name.title():15} | Start: {order.production_start_time.strftime('%H:%M')} | End: {order.production_end_time.strftime('%H:%M')} | Exp: {order.expiration_time.strftime('%H:%M')} | {status}")
    current_time = order.production_end_time  # move time forward
