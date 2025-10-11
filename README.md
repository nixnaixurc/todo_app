# Yet Another One To-Do App!

This **To-Do App** is a user-friendly task management tool designed to help individuals organize their daily activities efficiently. 
With a clean and intuitive interface, users can easily add and delete tasks, ensuring that nothing important is overlooked.

Key Features:
- Task Management: Users can create tasks with titles and descriptions, allowing for detailed tracking of responsibilities.
- Customizable Appearance: The application offers settings to customize background colors, button colors, and secondary colors, enabling users to personalize their experience.
- Persistent Storage: Tasks are saved to a local file, ensuring that users' lists are preserved even after closing the application.
- User -Friendly Interface: The layout is designed for ease of use, with clearly labeled buttons and input fields for quick task entry.
- Settings Management: Users can easily access a settings window to adjust the application's appearance or restore default settings.
- Visual Feedback: The application provides visual cues and warnings to guide users, such as alerts for invalid task entries or reminders to select a task before deletion.

Whether you're managing personal errands, work projects, or study schedules, the To-Do List Application is a versatile tool that helps you stay organized and productive.

# How to use

1) Download the code:
```
git clone https://github.com/triple0ero/todo_app.git
```
3) Install dependencies:
```
pip install -r requirements.txt
```
3) Start the app:
```
python3 main.py
```
4) Enjoy!



#include <stdio.h>

int main() {
    int result;  // Одна переменная для всех результатов
    
    printf("=== Калькулятор с повторным использованием переменных ===\n\n");
    
    // 1. Сложение
    result = 15 + 7;
    printf("1. Сложение: 15 + 7 = %d\n", result);
    
    // 2. Умножение (переиспользуем ту же переменную)
    result = 8 * 4;
    printf("2. Умножение: 8 * 4 = %d\n", result);
    
    // 3. Деление (снова используем ту же переменную)
    result = 100 / 5;
    printf("3. Деление: 100 / 5 = %d\n", result);
    
    // 4. Остаток от деления
    result = 17 % 5;
    printf("4. Остаток от деления: 17 %% 5 = %d\n", result);
    
    // 5. Инкремент (увеличиваем значение на 1)
    result++;
    printf("5. После инкремента: %d\n", result);
    
    // 6. Возведение в квадрат
    result = result * result;
    printf("6. Квадрат предыдущего значения: %d\n", result);
    
    // 7. Сброс до нуля
    result = 0;
    printf("7. Сброс значения: %d\n", result);
    
    // 8. Накопительная сумма (используем ту же переменную в цикле)
    printf("8. Накопительная сумма чисел от 1 до 5: ");
    for (int i = 1; i <= 5; i++) {
        result = result + i;  // Каждый раз используем старый результат
        printf("%d ", result);
    }
    printf("\n");
    
    // 9. Работа с пользовательским вводом
    int number;
    printf("\n9. Введите число: ");
    scanf("%d", &number);
    
    result = number * 2;  // Используем result для хранения удвоенного значения
    printf("   Удвоенное число: %d\n", result);
    
    result = result - 10;  // Используем тот же result для вычисления
    printf("   Минус 10: %d\n", result);
    
    result = result * result;  // И снова используем ту же переменную
    printf("   Квадрат результата: %d\n", result);
    
    printf("\nИтоговое значение переменной result: %d\n", result);
    
    return 0;
}