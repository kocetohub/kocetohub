import tkinter as tk
from tkinter import Entry, Toplevel, Label, StringVar, Button
from tkinter.ttk import Combobox
from datetime import datetime, timedelta

# Речник, който свързва марките с моделите
модели_на_автомобили = {
    "BMW": ["1 Серия", "3 Серия", "5 Серия", "X3", "X5"],
    "Audi": ["A3", "A4", "A6", "Q3", "Q5"],
    "Mercedes": ["A-Class", "C-Class", "E-Class", "GLC", "GLE"],
}

def обновяване_на_модели(event):
    избрана_марка = марка.get()
    модели = модели_на_автомобили.get(избрана_марка, [])  # Получаваме списък с модели за избраната марка
    комбо_модел.config(values=модели)  # Актуализираме стойностите в падащото меню за модели

def запази_данни():
    въведена_марка = марка.get()
    въведен_модел = комбо_модел.get()
    въведена_година = комбо_година.get()
    въведен_двигател = комбо_двигател.get()
    въведена_трансмисия = комбо_трансмисия.get()
    print(f"Марка: {въведена_марка}, Модел: {въведен_модел}, Година: {въведена_година}, Тип двигател: {въведен_двигател}, Трансмисия: {въведена_трансмисия}")

def отвори_прозорец_за_детайли_на_автомобила():
    прозорец_за_детайли = Toplevel(основен_прозорец)
    прозорец_за_детайли.title("Детайли за автомобила")
    прозорец_за_детайли.geometry("350x300")

    # Марка
    етикет_марка = Label(прозорец_за_детайли, text="Марка:")
    етикет_марка.grid(row=0, column=0, padx=10, pady=5)
    марки = list(модели_на_автомобили.keys())  # Използваме ключовете на речника като опции за марки
    global марка
    марка = StringVar(прозорец_за_детайли)
    комбо_марка = Combobox(прозорец_за_детайли, textvariable=марка, values=марки, state="readonly", font=("Arial", 12))
    комбо_марка.grid(row=0, column=1, padx=10, pady=5)
    комбо_марка.bind("<<ComboboxSelected>>", обновяване_на_модели)  # Промяна на моделите при избор на марка

    # Модел
    етикет_модел = Label(прозорец_за_детайли, text="Модел:")
    етикет_модел.grid(row=1, column=0, padx=10, pady=5)
    модели = модели_на_автомобили.get(марки[0], [])  # По подразбиране избираме моделите за първата марка
    global комбо_модел
    модел = StringVar(прозорец_за_детайли)
    комбо_модел = Combobox(прозорец_за_детайли, textvariable=модел, values=модели, state="readonly", font=("Arial", 12))
    комбо_модел.grid(row=1, column=1, padx=10, pady=5)

    # Година
    етикет_година = Label(прозорец_за_детайли, text="Година:")
    етикет_година.grid(row=2, column=0, padx=10, pady=5)
    години = ["2022", "2021", "2020", "2019", "2018", "2017", "2016", "2015", "2014", "2013", "2012", "2011"]  # Примерни опции за години
    global комбо_година
    година = StringVar(прозорец_за_детайли)
    комбо_година = Combobox(прозорец_за_детайли, textvariable=година, values=години, font=("Arial", 12))
    комбо_година.grid(row=2, column=1, padx=10, pady=5)

    # Тип двигател
    етикет_двигател = Label(прозорец_за_детайли, text="Тип двигател:")
    етикет_двигател.grid(row=3, column=0, padx=10, pady=5)
    типове_двигатели = ["Бензин", "Дизел", "Хибрид", "Електрически"]
    global комбо_двигател
    двигател = StringVar(прозорец_за_детайли)
    комбо_двигател = Combobox(прозорец_за_детайли, textvariable=двигател, values=типове_двигатели, font=("Arial", 12))
    комбо_двигател.grid(row=3, column=1, padx=10, pady=5)

    # Трансмисия
    етикет_трансмисия = Label(прозорец_за_детайли, text="Трансмисия:")
    етикет_трансмисия.grid(row=4, column=0, padx=10, pady=5)
    трансмисии = ["Ръчна", "Автоматична"]
    global комбо_трансмисия
    трансмисия = StringVar(прозорец_за_детайли)
    комбо_трансмисия = Combobox(прозорец_за_детайли, textvariable=трансмисия, values=трансмисии, font=("Arial", 12))
    комбо_трансмисия.grid(row=4, column=1, padx=10, pady=5)

    # Бутон за запазване
    бутон_запази = tk.Button(прозорец_за_детайли, text="Запази", command=запази_данни)
    бутон_запази.grid(row=5, column=0, columnspan=2, pady=20)

def отвори_подпрозорец(име):
    подпрозорец = Toplevel(основен_прозорец)
    подпрозорец.title(име)
    подпрозорец.geometry("300x200")
    етикет = tk.Label(подпрозорец, text=f"Това е подпрозорецът за: {име}", font=("Arial", 14))
    етикет.pack(pady=20)

def отвори_нов_прозорец():
    нов_прозорец = Toplevel(основен_прозорец)
    нов_прозорец.title("Автомобилен дневник")
    нов_прозорец.geometry("450x400")

    имена_на_бутони = [
        "автомобил", "дата", "текущи километри",
        "обслужване/ремонт", "технически преглед",
        "застраховка", "винетка", "данък", "други"
    ]

    for i, име in enumerate(имена_на_бутони, start=1):
        if име == "автомобил":
            бутон = tk.Button(нов_прозорец, text=име, command=отвори_прозорец_за_детайли_на_автомобила)
        elif име == "текущи километри":
            бутон = tk.Button(нов_прозорец, text=име, command=отвори_подпрозорец_километри)
        elif име == "обслужване/ремонт":
            бутон = tk.Button(нов_прозорец, text=име, command=отвори_подпрозорец_обслужване)
        elif име == "винетка":
            бутон = tk.Button(нов_прозорец, text=име, command=отвори_подпрозорец_винетка)
        elif име == "застраховка":
            бутон = tk.Button(нов_прозорец, text=име, command=отвори_подпрозорец_застраховка)
        elif име == "дата":  # Добавяме новия бутон за въвеждане на дата
            бутон = tk.Button(нов_прозорец, text=име, command=отвори_подпрозорец_дата)
        elif име == "технически преглед":
            бутон = tk.Button(нов_прозорец, text=име, command=отвори_подпрозорец_технически_преглед)
        elif име == "данък":
            бутон = tk.Button(нов_прозорец, text=име, command=отвори_подпрозорец_данък)

        else:
            бутон = tk.Button(нов_прозорец, text=име, command=lambda име=име: отвори_подпрозорец(име))
        ред = (i - 1) // 3
        колона = (i - 1) % 3
        бутон.grid(row=ред, column=колона, padx=10, pady=10)

def отвори_подпрозорец_данък():
    подпрозорец = Toplevel()
    подпрозорец.title("Въведи цена на данък")
    подпрозорец.geometry("300x150")

    етикет = Label(подпрозорец, text="Въведи цена на данък:", font=("Arial", 12))
    етикет.pack(pady=10)

    поле_данък = Entry(подпрозорец, font=("Arial", 12))
    поле_данък.pack(pady=10)

    бутон_запази = Button(подпрозорец, text="Запази", command=lambda: запази_данък(подпрозорец, поле_данък))
    бутон_запази.pack(pady=10)

def запази_данък(подпрозорец, поле_данък):
    въведен_данък = поле_данък.get()
    print(f"Цена на данъка: {въведен_данък}")
    подпрозорец.destroy()



def отвори_подпрозорец_технически_преглед():
    global прозорец_технически_преглед
    global поле_дата

    прозорец_технически_преглед = Toplevel(основен_прозорец)
    прозорец_технически_преглед.title("Технически преглед")
    прозорец_технически_преглед.geometry("300x200")

    етикет_дата = Label(прозорец_технически_преглед, text="Дата на последен технически преглед ДД.ММ.ГГГГ:")
    етикет_дата.pack(pady=10)

    поле_дата = tk.Entry(прозорец_технически_преглед)
    поле_дата.pack(pady=5)

    бутон_изчисли = tk.Button(прозорец_технически_преглед, text="Изчисли следващата дата", command=изчисли_следващ_преглед)
    бутон_изчисли.pack(pady=10)

def изчисли_следващ_преглед():
    global поле_дата

    въведена_дата = поле_дата.get()
    print(f"Въведена дата: {въведена_дата}")  # Тази линия ще изведе въведената дата в конзолата за отстраняване на грешки
    try:
        последен_преглед = datetime.strptime(въведена_дата, "%d.%m.%Y")
        следващ_преглед = последен_преглед + timedelta(days=365)  # Добавяме 1 година
        резултат = f"Следващата дата на технически преглед е: {следващ_преглед.strftime('%d.%m.%Y')}"
    except ValueError:
        резултат = "Моля, въведете валидна дата във формат ДД.ММ.ГГГГ."

    print(резултат)  # Тази линия ще изведе резултата в конзолата


def отвори_подпрозорец_дата():
    подпрозорец = Toplevel(основен_прозорец)
    подпрозорец.title("Въведи дата")
    подпрозорец.geometry("300x150")

    етикет = tk.Label(подпрозорец, text="Въведи дата във формат 'ДД.ММ.ГГГГ':", font=("Arial", 12))
    етикет.pack(pady=10)

    поле_дата = tk.Entry(подпрозорец, font=("Arial", 12))
    поле_дата.pack(pady=10)

    бутон_запази = tk.Button(подпрозорец, text="Запази", command=lambda: запази_дата(подпрозорец, поле_дата))
    бутон_запази.pack(pady=10)

def запази_дата(подпрозорец, поле_дата):
    въведена_дата = поле_дата.get()
    print(f"Текуща дата: {въведена_дата}")
    подпрозорец.destroy()

def отвори_подпрозорец_застраховка():
    подпрозорец = Toplevel(основен_прозорец)
    подпрозорец.title("Избери тип на застраховка")
    подпрозорец.geometry("300x150")

    етикет = tk.Label(подпрозорец, text="Избери тип на застраховка:", font=("Arial", 12))
    етикет.pack(pady=10)

    типове_застраховки = ["Гражданска отговорност", "Каско"]
    застраховка = StringVar(подпрозорец)
    комбо_застраховка = Combobox(подпрозорец, textvariable=застраховка, values=типове_застраховки, font=("Arial", 12))
    комбо_застраховка.pack(pady=10)

    бутон_запази = tk.Button(подпрозорец, text="Запази", command=lambda: запази_застраховка(подпрозорец, застраховка))
    бутон_запази.pack(pady=10)

def запази_застраховка(подпрозорец, застраховка):
    избрана_застраховка = застраховка.get()
    print(f"Избран тип на застраховка: {избрана_застраховка}")
    подпрозорец.destroy()


def отвори_подпрозорец_винетка():
    подпрозорец = Toplevel(основен_прозорец)
    подпрозорец.title("Избери тип на винетка")
    подпрозорец.geometry("300x150")

    етикет = tk.Label(подпрозорец, text="Избери тип на винетка:", font=("Arial", 12))
    етикет.pack(pady=10)

    типове_винетки = ["Годишна", "Тримесечна", "Месечна", "Седмична", "Уикенд"]
    винетка = StringVar(подпрозорец)
    комбо_винетка = Combobox(подпрозорец, textvariable=винетка, values=типове_винетки, font=("Arial", 12))
    комбо_винетка.pack(pady=10)

    бутон_запази = tk.Button(подпрозорец, text="Запази", command=lambda: запази_винетка(подпрозорец, винетка))
    бутон_запази.pack(pady=10)

def запази_винетка(подпрозорец, винетка):
    избрана_винетка = винетка.get()
    print(f"Избран тип на винетка: {избрана_винетка}")
    подпрозорец.destroy()


def отвори_подпрозорец_километри():
    подпрозорец = Toplevel(основен_прозорец)
    подпрозорец.title("Въвеждане на текущи километри")
    подпрозорец.geometry("350x200")

    етикет = tk.Label(подпрозорец, text="Въведете текущите километри на автомобила:", font=("Arial", 12))
    етикет.pack(pady=10)

    поле_за_въвеждане = tk.Entry(подпрозорец, font=("Arial", 12))
    поле_за_въвеждане.pack(pady=10)

    бутон_запази = tk.Button(подпрозорец, text="Запази", command=lambda: запази_километри(подпрозорец, поле_за_въвеждане))
    бутон_запази.pack(pady=10)

def отвори_подпрозорец_обслужване():
    подпрозорец = Toplevel(основен_прозорец)
    подпрозорец.title("Обслужване/Ремонт")
    подпрозорец.geometry("300x450")

    етикет = tk.Label(подпрозорец, text="Изберете тип обслужване или ремонт:", font=("Arial", 12))
    етикет.pack(pady=10)


    бутон_1 = tk.Button(подпрозорец, text="смяна на масло", command=lambda: изпълни_действие_1(подпрозорец))
    бутон_1.pack(pady=5)

    бутон_2 = tk.Button(подпрозорец, text="смяна на маслен филтър", command=lambda: изпълни_действие_2(подпрозорец))
    бутон_2.pack(pady=5)

    бутон_3 = tk.Button(подпрозорец, text="смяна на гуми", command=lambda: изпълни_действие_3(подпрозорец))
    бутон_3.pack(pady=5)

    бутон_4 = tk.Button(подпрозорец, text="смяна на накладки", command=lambda: изпълни_действие_4(подпрозорец))
    бутон_4.pack(pady=10)

    бутон_5 = tk.Button(подпрозорец, text="смяна на спирачни дискове", command=lambda: изпълни_действие_5(подпрозорец))
    бутон_5.pack(pady=5)

    бутон_6 = tk.Button(подпрозорец, text="смяна на водна помпа", command=lambda: изпълни_действие_6(подпрозорец))
    бутон_6.pack(pady=5)

    бутон_7 = tk.Button(подпрозорец, text="смяна на турбина", command=lambda: изпълни_действие_7(подпрозорец))
    бутон_7.pack(pady=5)

    бутон_8 = tk.Button(подпрозорец, text="смяна на антифриз", command=lambda: изпълни_действие_8(подпрозорец))
    бутон_8.pack(pady=5)

    бутон_9 = tk.Button(подпрозорец, text="смяна на горивни свещи", command=lambda: изпълни_действие_9(подпрозорец))
    бутон_9.pack(pady=5)

    бутон_10 = tk.Button(подпрозорец, text="смяна на запалителни бобина", command=lambda: изпълни_действие_10(подпрозорец))
    бутон_10.pack(pady=5)


# смяна на масло
def изпълни_действие_1(подпрозорец):
    # Функция за изчисление на пробега до следващата смяна на масло
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_масло = float(поле_последна_смяна_на_масло.get())
            интервал_смяна_масло = 10000  # Интервал за смяна на масло в километри
            следваща_смяна = интервал_смяна_масло -(текущ_пробег - последна_смяна_на_масло)
            print(f"До следващата смяна на масло остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на масло")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна смяна на масло
    label_последна_смяна_на_масло = tk.Label(root, text="Въведете последна смяна на масло в км:", font=("Arial", 12))
    label_последна_смяна_на_масло.pack(pady=10)

    поле_последна_смяна_на_масло = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_масло.pack(pady=10)

    # Бутон за изчисление на следваща смяна на масло
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на масло", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)

def запази_стойности_за_бутон_1(подпрозорец, поле_стойност_1):
    стойност_1 = поле_стойност_1.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_1}")
    подпрозорец.destroy()



# смяна на маслен филтър
def изпълни_действие_2(подпрозорец):
    # Функция за изчисление на пробега до следващата смяна на маслен филтър
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_маслен_филтър = float(поле_последна_смяна_на_маслен_филтър.get())
            интервал_смяна_на_маслен_филтър = 10000  # Интервал за смяна на маслен филтър в километри
            следваща_смяна = интервал_смяна_на_маслен_филтър -(текущ_пробег - последна_смяна_на_маслен_филтър)
            print(f"До следващата смяна на маслен филтър остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на маслен филтър")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна смяна на маслен филтър
    label_последна_смяна_на_маслен_филтър = tk.Label(root, text="Въведете последна смяна на маслен филтър в км:", font=("Arial", 12))
    label_последна_смяна_на_маслен_филтър.pack(pady=10)

    поле_последна_смяна_на_маслен_филтър = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_маслен_филтър.pack(pady=10)

    # Бутон за изчисление на следваща смяна на маслен филтър
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на маслен филтър", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)

def запази_стойности_за_бутон_2(подпрозорец, поле_стойност_2):
    стойност_2 = поле_стойност_2.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_2}")
    подпрозорец.destroy()



# смяна на гуми
def изпълни_действие_3(подпрозорец):
    # Функция за изчисление на пробега до следващата смяна на гуми
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_гуми = float(поле_последна_смяна_на_гуми.get())
            интервал_смяна_на_гуми = 40000  # Интервал за смяна на гуми в километри
            следваща_смяна = интервал_смяна_на_гуми -(текущ_пробег - последна_смяна_на_гуми)
            print(f"До следващата смяна на гуми остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на гуми")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна смяна на гуми
    label_последна_смяна_на_гуми = tk.Label(root, text="Въведете последна смяна на гуми в км:", font=("Arial", 12))
    label_последна_смяна_на_гуми.pack(pady=10)

    поле_последна_смяна_на_гуми = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_гуми.pack(pady=10)

    # Бутон за изчисление на следваща смяна на гуми
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на гуми", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)

def запази_стойности_за_бутон_3(подпрозорец, поле_стойност_3):
    стойност_3 = поле_стойност_3.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_3}")
    подпрозорец.destroy()


# смяна на накладки
def изпълни_действие_4(подпрозорец):
    # Функция за изчисление на пробега до следващата  Смяна на накладки
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_накладки = float(поле_последна_смяна_на_накладки.get())
            интервал_смяна_на_накладки = 25000  # Интервал за Смяна на накладки в километри
            следваща_смяна = интервал_смяна_на_накладки - (текущ_пробег - последна_смяна_на_накладки)
            print(f"До следващата смяна на накладки остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на накладки")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна Смяна на накладки
    label_последна_смяна_на_накладки = tk.Label(root, text="Въведете последна смяна на накладки в км:", font=("Arial", 12))
    label_последна_смяна_на_накладки.pack(pady=10)

    поле_последна_смяна_на_накладки = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_накладки.pack(pady=10)

    # Бутон за изчисление на следваща смяна на накладки
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на накладки", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)


def запази_стойности_за_бутон_4(подпрозорец, поле_стойност_4):
    стойност_4 = поле_стойност_4.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_4}")
    подпрозорец.destroy()



# смяна на спирачни дискове
def изпълни_действие_5(подпрозорец):
    # Функция за изчисление на пробега до следващата  смяна на спирачни дискове
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_спирачни_дискове = float(поле_последна_смяна_на_спирачни_дискове.get())
            интервал_смяна_на_спирачни_дискове = 70000  # Интервал за смяна_на_спирачни_дискове в километри
            следваща_смяна = интервал_смяна_на_спирачни_дискове - (текущ_пробег - последна_смяна_на_спирачни_дискове)
            print(f"До следващата смяна на спирачни дискове  {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на спирачни дискове")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна Смяна на спирачни дискове
    label_последна_смяна_на_спирачни_дискове = tk.Label(root, text="Въведете последна смяна на спирачни дискове в км:", font=("Arial", 12))
    label_последна_смяна_на_спирачни_дискове.pack(pady=10)

    поле_последна_смяна_на_спирачни_дискове = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_спирачни_дискове.pack(pady=10)

    # Бутон за изчисление на следваща смяна на спирачни дискове
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на спирачни дискове", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)


def запази_стойности_за_бутон_5(подпрозорец, поле_стойност_5):
    стойност_5 = поле_стойност_5.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_5}")
    подпрозорец.destroy()


# смяна_на_водна_помпа
def изпълни_действие_6(подпрозорец):
    # Функция за изчисление на пробега до следващата  смяна на водна помпа
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_водна_помпа = float(поле_последна_смяна_на_водна_помпа.get())
            интервал_смяна_на_водна_помпа = 100000  # Интервал за смяна на водна помпа в километри
            следваща_смяна = интервал_смяна_на_водна_помпа - (текущ_пробег - последна_смяна_на_водна_помпа)
            print(f"До следващата смяна на водна помпа остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на водна помпа")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна смяна на водна помпа
    label_последна_смяна_на_водна_помпа = tk.Label(root, text="Въведете последна смяна на водна помпа в км:", font=("Arial", 12))
    label_последна_смяна_на_водна_помпа.pack(pady=10)

    поле_последна_смяна_на_водна_помпа = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_водна_помпа.pack(pady=10)

    # Бутон за изчисление на следваща смяна на водна помпа
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на водна помпа", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)


def запази_стойности_за_бутон_6(подпрозорец, поле_стойност_6):
    стойност_6 = поле_стойност_6.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_6}")
    подпрозорец.destroy()


# смяна на турбина
def изпълни_действие_7(подпрозорец):
    # Функция за изчисление на пробега до следващата смяна на турбина
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_турбина = float(поле_последна_смяна_на_турбина.get())
            интервал_смяна_на_турбина = 120000  # Интервал за смяна на турбина в километри
            следваща_смяна = интервал_смяна_на_турбина - (текущ_пробег - последна_смяна_на_турбина)
            print(f"До следващата смяна на турбина остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на турбина")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна смяна на турбина
    label_последна_смяна_на_турбина = tk.Label(root, text="Въведете последна смяна на турбина в км:", font=("Arial", 12))
    label_последна_смяна_на_турбина.pack(pady=10)

    поле_последна_смяна_на_турбина = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_турбина.pack(pady=10)

    # Бутон за изчисление на следваща смяна на турбина
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на турбина", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)

def запази_стойности_за_бутон_7(подпрозорец, поле_стойност_7):
    стойност_7 = поле_стойност_7.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_7}")
    подпрозорец.destroy()


# смяна на антифриз
def изпълни_действие_8(подпрозорец):
    # Функция за изчисление на пробега до следващата смяна на антифриз
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_антифриз = float(поле_последна_смяна_на_антифриз.get())
            интервал_смяна_на_антифриз = 50000  # Интервал за смяна_на_антифриз в километри
            следваща_смяна = интервал_смяна_на_антифриз - (текущ_пробег - последна_смяна_на_антифриз)
            print(f"До следващата смяна на антифриза остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на антифриз")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна смяна на антифриз
    label_последна_смяна_на_антифриз = tk.Label(root, text="Въведете последна смяна на антифриз в км:", font=("Arial", 12))
    label_последна_смяна_на_антифриз.pack(pady=10)

    поле_последна_смяна_на_антифриз = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_антифриз.pack(pady=10)

    # Бутон за изчисление на следваща смяна на антифриз
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на антифриз", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)

def запази_стойности_за_бутон_8(подпрозорец, поле_стойност_8):
    стойност_8 = поле_стойност_8.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_8}")
    подпрозорец.destroy()


# смяна на горивни свещи
def изпълни_действие_9(подпрозорец):
    # Функция за изчисление на пробега до следващата смяна на гуми
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_горивни_свещи = float(поле_последна_смяна_на_горивни_свещи.get())
            интервал_смяна_на_горивни_свещи = 15000  # Интервал за смяна на горивни свещи в километри
            следваща_смяна = интервал_смяна_на_горивни_свещи -(текущ_пробег - последна_смяна_на_горивни_свещи)
            print(f"До следващата смяна на горивни свещи остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на горивни свещи")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна смяна на горивни свещи
    label_последна_смяна_на_горивни_свещи = tk.Label(root, text="Въведете последна смяна на горивни свещи в км:", font=("Arial", 12))
    label_последна_смяна_на_горивни_свещи.pack(pady=10)

    поле_последна_смяна_на_горивни_свещи = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_горивни_свещи.pack(pady=10)

    # Бутон за изчисление на следваща смяна на горивни свещи
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на горивни свещи", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)

def запази_стойности_за_бутон_9(подпрозорец, поле_стойност_9):
    стойност_9 = поле_стойност_9.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_9}")
    подпрозорец.destroy()


# смяна на запалителна бобина
def изпълни_действие_10(подпрозорец):
    # Функция за изчисление на пробега до следващата смяна на горивни свещи
    def изчисли_пробег():
        try:
            текущ_пробег = float(поле_пробег.get())
            последна_смяна_на_запалителни_бобина = float(поле_последна_смяна_на_запалителни_бобина.get())
            интервал_смяна_на_запалителни_бобина = 20000  # Интервал за смяна_на_запалителни_бобина в километри
            следваща_смяна = интервал_смяна_на_запалителни_бобина - (текущ_пробег - последна_смяна_на_запалителни_бобина)
            print(f"До следващата смяна на запалителна бобина остават {следваща_смяна:.2f} км.")
        except ValueError:
            print("Грешка: Моля, въведете коректен числов пробег.")

    # Създаване на основен прозорец
    root = tk.Tk()
    root.title("Изчисление на пробег за смяна на запалителна бобина")

    # Лейбъл и поле за въвеждане на текущ пробег
    label_пробег = tk.Label(root, text="Въведете текущ пробег в км:", font=("Arial", 12))
    label_пробег.pack(pady=10)

    поле_пробег = tk.Entry(root, font=("Arial", 12))
    поле_пробег.pack(pady=10)

    # Лейбъл и поле за въвеждане на последна смяна на запалителна бобина
    label_последна_смяна_на_запалителни_бобина = tk.Label(root, text="Въведете последна смяна на запалителна бобина в км:", font=("Arial", 12))
    label_последна_смяна_на_запалителни_бобина.pack(pady=10)

    поле_последна_смяна_на_запалителни_бобина = tk.Entry(root, font=("Arial", 12))
    поле_последна_смяна_на_запалителни_бобина.pack(pady=10)

    # Бутон за изчисление на следваща смяна на запалителна бобина
    бутон_изчисли = tk.Button(root, text="Изчисли следваща смяна на запалителна бобина", command=изчисли_пробег)
    бутон_изчисли.pack(pady=10)

def запази_стойности_за_бутон_10(подпрозорец, поле_стойност_10):
    стойност_10 = поле_стойност_10.get()
    print(f"Километри при предишна смяна/ремонт: {стойност_10}")
    подпрозорец.destroy()


def запази_километри(подпрозорец, поле_за_въвеждане):
    километри = поле_за_въвеждане.get()
    print(f"Текущи километри: {километри}")
    подпрозорец.destroy()

def запази_обслужване(подпрозорец, обслужване):
    избрано_обслужване = обслужване.get()
    print(f"Избрано обслужване/ремонт: {избрано_обслужване}")
    подпрозорец.destroy()

# Създаване на основен прозорец
основен_прозорец = tk.Tk()
основен_прозорец.title("Автомобилен дневник")
основен_прозорец.geometry("400x400")

бутон_нов_прозорец = tk.Button(основен_прозорец, text="Добре дошли", command=отвори_нов_прозорец)
бутон_нов_прозорец.pack(pady=20)

основен_прозорец.mainloop()
