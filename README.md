# light
The light turns on when we move the mouse on the screen.

import tkinter as tk

# تابع عامل واکنشی ساده
def smart_street_light(percept):
    if percept == "movement_detected":
        return "turn_on_light"
    elif percept == "no_movement":
        return "turn_off_light"
    else:
        return "do_nothing"

def update_light(percept):
    action = smart_street_light(percept)
    if action == "turn_on_light":
        canvas.itemconfig(light_circle, fill="yellow")
        status_label.config(text="حرکت شناسایی شد 💡 چراغ روشن است", fg="gold")
    elif action == "turn_off_light":
        canvas.itemconfig(light_circle, fill="gray")
        status_label.config(text="حرکتی نیست 🌙 چراغ خاموش است", fg="gray")

# تابعی که با حرکت موس صدا زده می‌شود
def on_mouse_move(event):
    global last_motion_time
    last_motion_time = window.after_cancel(last_motion_time) if last_motion_time else None
    update_light("movement_detected")
    start_motion_timer()

# تابعی برای خاموش کردن چراغ در صورت نبود حرکت
def start_motion_timer():
    global last_motion_time
    # بعد از 3 ثانیه بدون حرکت، چراغ خاموش می‌شود
    last_motion_time = window.after(3000, lambda: update_light("no_movement"))

# ایجاد پنجره اصلی
window = tk.Tk()
window.title("عامل واکنشی ساده - چراغ هوشمند با حرکت موس")
window.geometry("320x380")

# بوم برای چراغ
canvas = tk.Canvas(window, width=200, height=200, bg="black")
canvas.pack(pady=20)

# دایره‌ی چراغ
light_circle = canvas.create_oval(50, 50, 150, 150, fill="gray")

# برچسب وضعیت
status_label = tk.Label(window, text="حرکتی نیست 🌙 چراغ خاموش است", font=("B Nazanin", 12))
status_label.pack(pady=10)

# اتصال رویداد حرکت موس به تابع
window.bind("<Motion>", on_mouse_move)

# شروع تایمر اولیه برای خاموش شدن چراغ
last_motion_time = None
start_motion_timer()

# اجرای برنامه
window.mainloop()
