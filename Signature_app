import tkinter as tk
from tkinter import messagebox
import random
import hashlib

# Elliptic Curve parameters
a = 2
b = 2
p = 17  # Prime modulus
G = (5, 1)  # Base point
n = 19  # Order of G, a prime number

# Modular arithmetic functions
def inverse_modulo(a, m):
    for i in range(1, m):
        if (a * i) % m == 1:
            return i
    raise ValueError(f"Inverse modulo of {a} does not exist modulo {m}")

def add_points(p1, p2):
    if p1 == "O":
        return p2
    if p2 == "O":
        return p1
    x1, y1 = p1
    x2, y2 = p2
    if x1 == x2 and (y1 != y2 or y1 == 0):
        return "O"
    if p1 != p2:
        m = ((y2 - y1) * inverse_modulo(x2 - x1, p)) % p
    else:
        m = ((3 * x1**2 + a) * inverse_modulo(2 * y1, p)) % p
    x3 = (m**2 - x1 - x2) % p
    y3 = (m * (x1 - x3) - y1) % p
    return (x3, y3)

def multiply_point(k, p):
    result = "O"
    for _ in range(k):
        result = add_points(result, p)
    return result

def generate_key_pair():
    private_key = random.randint(1, n - 1)
    public_key = multiply_point(private_key, G)
    return private_key, public_key

def hash_message(message):
    return int.from_bytes(hashlib.sha256(message.encode()).digest(), byteorder='big') % n

def generate_signature(private_key, message):
    k = random.randint(1, n - 1)
    R = multiply_point(k, G)
    r = R[0] % n
    s = (inverse_modulo(k, n) * (hash_message(message) + private_key * r)) % n
    return (r, s)

def verify_signature(public_key, hashed_message, signature):
    r, s = signature
    if not (0 < r < n and 0 < s < n):
        return False
    w = inverse_modulo(s, n)
    u1 = (hashed_message * w) % n
    u2 = (r * w) % n
    P = add_points(multiply_point(u1, G), multiply_point(u2, public_key))
    if P == "O":
        return False
    return P[0] % n == r

def create_signature_page(root):
    def sign_message():
        message = message_entry.get()
        if message:
            hashed_message = hash_message(message)
            hash_label.config(text="Mesajın hash değeri: " + str(hashed_message))
            if hashed_message not in messages:
                messages[hashed_message] = message
            signature = generate_signature(private_key, message)
            signature_label.config(text="İmza: " + str(signature))
        else:
            messagebox.showerror("Hata", "Lütfen bir mesaj girin.")

    root.withdraw()

    sign_page = tk.Toplevel(root)
    sign_page.title("İmza Oluşturma")
    sign_page.geometry("400x250")  # Height increased to accommodate hash label

    message_label = tk.Label(sign_page, text="Mesajı girin:")
    message_label.pack(pady=10)

    message_entry = tk.Entry(sign_page, width=30)
    message_entry.pack()

    sign_button = tk.Button(sign_page, text="Mesajı İmzala", command=sign_message)
    sign_button.pack(pady=10)

    hash_label = tk.Label(sign_page, text="")
    hash_label.pack()

    signature_label = tk.Label(sign_page, text="")
    signature_label.pack()

    def back_to_main():
        sign_page.destroy()
        root.deiconify()

    back_button = tk.Button(sign_page, text="Ana Sayfaya Dön", command=back_to_main)
    back_button.pack(pady=10)

def verify_signature_page(root):
    def verify_signature_func():
        hash_value = hash_entry.get()
        signature_input = signature_entry.get()
        if hash_value and signature_input:
            hash_value = int(hash_value)
            signature = tuple(map(int, signature_input.strip("()").split(",")))
            if hash_value in messages:
                valid = verify_signature(public_key, hash_value, signature)
                if valid:
                    message_label.config(text="Mesaj: " + messages[hash_value])
                    messagebox.showinfo("Başarılı", "İmza doğru")
                else:
                    messagebox.showerror("Hata", "İmza yanlış")
            else:
                messagebox.showerror("Hata", "Geçersiz hash değeri")
        else:
            messagebox.showerror("Hata", "Lütfen hash ve imza alanlarını doldurun.")

    root.withdraw()

    verify_page = tk.Toplevel(root)
    verify_page.title("İmza Doğrulama")
    verify_page.geometry("400x300")  # Height increased to accommodate message label

    hash_label = tk.Label(verify_page, text="Hash değeri girin:")
    hash_label.pack(pady=10)

    hash_entry = tk.Entry(verify_page, width=30)
    hash_entry.pack()

    signature_label = tk.Label(verify_page, text="İmza:")
    signature_label.pack()

    signature_entry = tk.Entry(verify_page, width=30)
    signature_entry.pack()

    verify_button = tk.Button(verify_page, text="İmzayı Doğrula", command=verify_signature_func)
    verify_button.pack(pady=10)

    message_label = tk.Label(verify_page, text="")
    message_label.pack()

    def back_to_main():
        verify_page.destroy()
        root.deiconify()

    back_button = tk.Button(verify_page, text="Ana Sayfaya Dön", command=back_to_main)
    back_button.pack(pady=10)

def choose_operation(operation):
    if operation == "Oluştur":
        create_signature_page(root)
    elif operation == "Doğrula":
        verify_signature_page(root)

# Generate key pair
private_key, public_key = generate_key_pair()

# Create messages
messages = {}

# Create GUI
root = tk.Tk()
root.title("ECC İmza Uygulaması")
root.geometry("300x300")  # Height increased to accommodate message label

operation_label = tk.Label(root, text="İmza İşlemi Seçin:")
operation_label.pack(pady=10)

create_button = tk.Button(root, text="İmza Oluştur", command=lambda: choose_operation("Oluştur"))
create_button.pack(pady=5)

verify_button = tk.Button(root, text="İmza Doğrula", command=lambda: choose_operation("Doğrula"))
verify_button.pack(pady=5)

root.mainloop()
