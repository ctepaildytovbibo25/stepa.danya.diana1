```# stepa.danya.diana1
наши работы
# Таблица умножения от 1 до 10
for i in range(1, 11):
    for j in range(1, 11):
        print(f"{i} × {j} = {i * j}")
    print("-" * 20)
```
```import tkinter as tk
from tkinter import font

class iPhoneCalculator:
    def __init__(self, root):
        self.root = root
        self.root.title("iPhone Calculator")
        self.root.configure(bg='#000000')
        self.root.resizable(False, False)
        
        # Переменные
        self.current_input = "0"
        self.previous_value = None
        self.operation = None
        self.reset_on_next_input = False
        
        # Настройка шрифтов
        self.display_font = font.Font(family="Helvetica", size=36, weight="normal")
        self.button_font = font.Font(family="Helvetica", size=24, weight="normal")
        
        self.create_widgets()
        
    def create_widgets(self):
        # Display
        self.display = tk.Label(
            self.root, 
            text=self.current_input, 
            font=self.display_font,
            bg='#000000',
            fg='#FFFFFF',
            anchor='e',
            padx=20
        )
        self.display.pack(fill='x', pady=(50, 20))
        
        # Кнопки
        buttons = [
            ['C', '±', '%', '÷'],
            ['7', '8', '9', '×'],
            ['4', '5', '6', '-'],
            ['1', '2', '3', '+'],
            ['0', '.', '=']
        ]
        
        for row_idx, row in enumerate(buttons):
            button_frame = tk.Frame(self.root, bg='#000000')
            button_frame.pack(fill='x', padx=5)
            
            for col_idx, button_text in enumerate(row):
                if button_text == '0':
                    # Особый случай для кнопки 0
                    btn = tk.Button(
                        button_frame,
                        text=button_text,
                        font=self.button_font,
                        bg=self.get_button_color(button_text),
                        fg=self.get_text_color(button_text),
                        border=0,
                        height=1,
                        width=5,
                        command=lambda b=button_text: self.on_button_click(b)
                    )
                    btn.pack(side='left', fill='x', expand=True, padx=2, pady=2)
                else:
                    btn = tk.Button(
                        button_frame,
                        text=button_text,
                        font=self.button_font,
                        bg=self.get_button_color(button_text),
                        fg=self.get_text_color(button_text),
                        border=0,
                        height=2,
                        width=5,
                        command=lambda b=button_text: self.on_button_click(b)
                    )
                    btn.pack(side='left', fill='x', expand=True, padx=2, pady=2)
    
    def get_button_color(self, text):
        if text in ['C', '±', '%']:
            return '#A5A5A5'  # Серый
        elif text in ['÷', '×', '-', '+', '=']:
            return '#FF9500'  # Оранжевый
        else:
            return '#333333'  # Темно-серый
    
    def get_text_color(self, text):
        if text in ['C', '±', '%']:
            return '#000000'  # Черный
        else:
            return '#FFFFFF'  # Белый
    
    def on_button_click(self, button_text):
        if button_text in '0123456789':
            self.handle_digit(button_text)
        elif button_text == '.':
            self.handle_decimal()
        elif button_text in ['+', '-', '×', '÷']:
            self.handle_operation(button_text)
        elif button_text == '=':
            self.handle_equals()
        elif button_text == 'C':
            self.handle_clear()
        elif button_text == '±':
            self.handle_negate()
        elif button_text == '%':
            self.handle_percentage()
        
        self.update_display()
    
    def handle_digit(self, digit):
        if self.reset_on_next_input:
            self.current_input = digit
            self.reset_on_next_input = False
        else:
            if self.current_input == "0":
                self.current_input = digit
            else:
                self.current_input += digit
    
    def handle_decimal(self):
        if '.' not in self.current_input:
            self.current_input += '.'
    
    def handle_operation(self, op):
        if self.previous_value is not None and not self.reset_on_next_input:
            self.calculate()
        
        self.previous_value = float(self.current_input)
        self.operation = op
        self.reset_on_next_input = True
    
    def handle_equals(self):
        if self.previous_value is not None and self.operation is not None:
            self.calculate()
            self.operation = None
            self.reset_on_next_input = True
    
    def handle_clear(self):
        self.current_input = "0"
        self.previous_value = None
        self.operation = None
        self.reset_on_next_input = False
    
    def handle_negate(self):
        try:
            value = float(self.current_input)
            self.current_input = str(-value)
        except:
            pass
    
    def handle_percentage(self):
        try:
            value = float(self.current_input)
            self.current_input = str(value / 100)
        except:
            pass
    
    def calculate(self):
        try:
            current_value = float(self.current_input)
            
            if self.operation == '+':
                result = self.previous_value + current_value
            elif self.operation == '-':
                result = self.previous_value - current_value
            elif self.operation == '×':
                result = self.previous_value * current_value
            elif self.operation == '÷':
                if current_value == 0:
                    self.current_input = "Error"
                    return
                result = self.previous_value / current_value
            
            # Форматирование результата
            if result.is_integer():
                self.current_input = str(int(result))
            else:
                self.current_input = str(round(result, 10)).rstrip('0').rstrip('.')
            
            self.previous_value = result
            
        except:
            self.current_input = "Error"
    
    def update_display(self):
        # Ограничение длины отображаемого текста
        if len(self.current_input) > 12:
            display_text = self.current_input[:12] + "..."
        else:
            display_text = self.current_input
        
        self.display.config(text=display_text)

def main():
    root = tk.Tk()
    root.configure(bg='#000000')
    
    # Установка размера окна и положения
    window_width = 350
    window_height = 600
    screen_width = root.winfo_screenwidth()
    screen_height = root.winfo_screenheight()
    x = (screen_width - window_width) // 2
    y = (screen_height - window_height) // 2
    root.geometry(f'{window_width}x{window_height}+{x}+{y}')
    
    calculator = iPhoneCalculator(root)
    root.mainloop()

if __name__ == "__main__":
    main()
```
