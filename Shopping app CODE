import tkinter as tk
from tkinter import messagebox, simpledialog
from datetime import datetime

class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price

class ShoppingCart:
    def __init__(self):
        self.items = []

    def add_item(self, product, quantity):
        self.items.append({"product": product, "quantity": quantity})

    def calculate_total(self):
        total = 0
        for item in self.items:
            total += item["product"].price * item["quantity"]
        return total

class OrderHistoryApp:
    def __init__(self, master, order_history):
        self.master = master
        self.master.title("Order History")

        self.order_history = order_history

        self.create_widgets()

    def create_widgets(self):
        self.label_order_history = tk.Label(self.master, text="Order History", font=("Helvetica", 18, "bold"))
        self.label_order_history.pack(pady=10)

        self.text_order_history = tk.Text(self.master, height=15, width=50)
        self.text_order_history.pack(pady=10)

        self.update_order_history()

    def update_order_history(self):
        self.text_order_history.delete(1.0, tk.END)
        for order in self.order_history:
            date_str = datetime.utcfromtimestamp(order["date"]).strftime('%Y-%m-%d %H:%M:%S')
            order_str = f"Date: {date_str}\n"
            for item in order["items"]:
                order_str += f"{item['product'].name} - Quantity: {item['quantity']}\n"
            order_str += f"Total: ${order['total']:.2f}\n\n"
            self.text_order_history.insert(tk.END, order_str)

class CheckoutApp:
    def __init__(self, master, shopping_app):
        self.master = master
        self.master.title("Checkout")

        self.gada_shopping_app = shopping_app
        self.gada_cart = shopping_app.gada_cart
        self.gada_current_user = shopping_app.gada_current_user
        self.gada_order_history = shopping_app.gada_order_history

        self.create_widgets()

    def create_widgets(self):
        self.label_discount = tk.Label(self.master, text="Discount Code:")
        self.label_discount.pack(pady=10)

        self.entry_discount = tk.Entry(self.master)
        self.entry_discount.pack()

        self.button_apply_discount = tk.Button(self.master, text="Apply Discount", command=self.apply_discount)
        self.button_apply_discount.pack(pady=5)

        self.label_shipping = tk.Label(self.master, text="Shipping and Tracking")
        self.label_shipping.pack(pady=10)

        self.label_shipping_date = tk.Label(self.master, text="Shipping Date: TBD")
        self.label_shipping_date.pack(pady=5)

        self.label_tracking_number = tk.Label(self.master, text="Tracking Number: TBD")
        self.label_tracking_number.pack(pady=5)

        self.label_summary = tk.Label(self.master, text="Checkout Summary")
        self.label_summary.pack(pady=10)

        cart_total = self.gada_cart.calculate_total()
        cart_items = "\n".join([f"{item['product'].name} - Quantity: {item['quantity']}" for item in self.gada_cart.items])

        self.label_cart_items = tk.Label(self.master, text=f"Shopping Cart:\n{cart_items}")
        self.label_cart_items.pack(pady=10)

        self.label_total = tk.Label(self.master, text=f"Total: ${cart_total:.2f}")
        self.label_total.pack(pady=10)

        self.button_confirm_order = tk.Button(self.master, text="Confirm Order", command=self.confirm_order)
        self.button_confirm_order.pack(pady=10)

    def apply_discount(self):
        discount_code = self.entry_discount.get().strip().upper()

        if discount_code == "FINAL":
            cart_total = self.gada_cart.calculate_total()
            discount_amount = 0.1 * cart_total
            discounted_total = cart_total - discount_amount

            messagebox.showinfo("Discount Applied", f"Discount code '{discount_code}' applied!\nDiscount Amount: ${discount_amount:.2f}")

            self.label_total.config(text=f"Total (After Discount): ${discounted_total:.2f}")
        else:
            messagebox.showerror("Invalid Discount Code", "Invalid discount code. Please enter a valid code.")

    def confirm_order(self):
        date = datetime.now().timestamp()
        order = {
            "date": date,
            "items": self.gada_cart.items.copy(),
            "total": self.gada_cart.calculate_total()
        }
        self.gada_order_history.append(order)

        messagebox.showinfo("Order Confirmed", "Your order has been confirmed!\nThank you for shopping with us.")

        self.gada_cart.items = []

        self.master.destroy()

class GadaShoppingApp:
    def __init__(self, master):
        self.master = master
        self.master.title("Interactive Shopping Cart")

        self.products = [
            Product("Laptop", 999.99),
            Product("Phone", 499.99),
            Product("Headphones", 99.99),
            Product("Smartwatch", 199.99),
            Product("Camera", 299.99),
            Product("Tablet", 349.99),
            Product("Printer", 129.99),
            Product("External Hard Drive", 79.99),
            Product("Bluetooth Speaker", 59.99),
            Product("Wireless Mouse", 29.99),
        ]

        self.gada_cart = ShoppingCart()
        self.gada_current_user = None
        self.gada_order_history = []

        self.create_widgets()

    def create_widgets(self):
        self.button_add_to_cart = tk.Button(self.master, text="Add to Cart", command=self.add_to_cart)
        self.button_add_to_cart.pack(pady=10)

        self.button_view_cart = tk.Button(self.master, text="View Cart", command=self.view_cart)
        self.button_view_cart.pack(pady=10)

        self.button_checkout = tk.Button(self.master, text="Checkout", command=self.checkout)
        self.button_checkout.pack(pady=10)

        self.label_products = tk.Label(self.master, text="Select items to add to your cart:")
        self.label_products.pack(pady=10)

        self.checkbox_vars = []
        self.checkboxes = []

        for product in self.products:
            var = tk.IntVar()
            checkbox = tk.Checkbutton(self.master, text=f"{product.name} - ${product.price}", variable=var)
            self.checkbox_vars.append(var)
            self.checkboxes.append(checkbox)
            checkbox.pack()

        self.button_order_history = tk.Button(self.master, text="Order History", command=self.view_order_history)
        self.button_order_history.pack(pady=10)

        self.label_account = tk.Label(self.master, text="Account:")
        self.label_account.pack(pady=5)

        self.button_sign_in = tk.Button(self.master, text="Sign In", command=self.sign_in)
        self.button_sign_in.pack(pady=5)

        self.button_sign_up = tk.Button(self.master, text="Sign Up", command=self.sign_up)
        self.button_sign_up.pack(pady=5)

        self.label_company_name = tk.Label(self.master, text="Gada Electronics", font=("Helvetica", 18, "bold"), fg="red")
        self.label_company_name.pack(side="left", padx=10, pady=10)

    def sign_in(self):
        user_accounts = {}
        email = simpledialog.askstring("Sign In", "Enter your email:")
        password = simpledialog.askstring("Sign In", "Enter your password:", show="*")

        email = email.lower().strip()

        if email in user_accounts and user_accounts[email]["password"] == password:
            self.gada_current_user = email
            messagebox.showinfo("Sign In", f"Welcome, {email}!")
        else:
            messagebox.showerror("Sign In Failed", "Invalid email or password.")

    def sign_up(self):
        user_accounts = {}
        email = simpledialog.askstring("Sign Up", "Enter your email:")
        password = simpledialog.askstring("Sign Up", "Create a password:", show="*")

        if email and password:
            email = email.lower().strip()
            user_accounts[email] = {"email": email, "password": password}
            self.gada_current_user = email
            messagebox.showinfo("Sign Up", "Account created successfully!")
        else:
            messagebox.showerror("Sign Up Failed", "Email and password are required.")

    def add_to_cart(self):
        for i, checkbox_var in enumerate(self.checkbox_vars):
            if checkbox_var.get():
                self.gada_cart.add_item(self.products[i], 1)

        messagebox.showinfo("Added to Cart", "Items added to your cart.")

    def view_cart(self):
        cart_total = self.gada_cart.calculate_total()
        cart_items = "\n".join([f"{item['product'].name} - Quantity: {item['quantity']}" for item in self.gada_cart.items])

        message = f"Shopping Cart:\n{cart_items}\nTotal: ${cart_total:.2f}"
        messagebox.showinfo("Your Cart", message)

    def checkout(self):
        checkout_window = tk.Toplevel(self.master)
        checkout_app = CheckoutApp(checkout_window, self)

    def view_order_history(self):
        if not self.gada_order_history:
            messagebox.showinfo("Order History", "No order history available.")
        else:
            order_history_window = tk.Toplevel(self.master)
            order_history_app = OrderHistoryApp(order_history_window, self.gada_order_history)

if __name__ == "__main__":
    root = tk.Tk()
    app = GadaShoppingApp(root)
    root.geometry("800x600")  # Set a fixed window size for demonstration purposes
    root.mainloop()
