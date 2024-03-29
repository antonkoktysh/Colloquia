# Коллоквиум по ОС от 26.10.2023
## Вариант 4
### <ins> Нулевая группа вопросов </ins>
#### 1. Win API, необходимое для решения Лабораторной работы номер 2
Для решения Лабораторной работы номер 2, необходимо использовать следующие функции из WinAPI:

Для создания потоков: _beginthreadex: Создание нового потока. Принимает указатель на функцию, которую поток должен выполнить. _endthreadex: Завершение потока. Принимает код завершения потока. 

Пример использования:


    #include <process.h>

    // Создание потока
    HANDLE hThread = (HANDLE)_beginthreadex(nullptr, 0, ThreadFunction, param, 0, nullptr);
Для работы с потоками:
WaitForSingleObject: Ожидание завершения работы объекта (в данном случае, потока). Принимает дескриптор объекта и время ожидания.

Пример использования:


    // Ожидание завершения потока
    WaitForSingleObject(hThread, INFINITE);
    Для засыпания:
Sleep: Приостановка выполнения программы на указанное количество миллисекунд.

Пример использования:

    // Засыпание на 100 миллисекунд
    Sleep(100);

#### 2. Процесс

Определение: Процесс в операционной системе Windows представляет собой экземпляр исполняемой программы. Каждый процесс обладает своим пространством памяти, в котором хранятся данные, инструкции и ресурсы, а также набором системных ресурсов, таких как дескрипторы файлов, описатели устройств и другие атрибуты.

Пример использования:
#include <windows.h>

int main() {
STARTUPINFO si;
PROCESS_INFORMATION pi;

    ZeroMemory(&si, sizeof(si));
    si.cb = sizeof(si);
    ZeroMemory(&pi, sizeof(pi));

    // Запуск нового процесса 
    if (CreateProcess(
        TEXT("notepad.exe"), // Имя исполняемого файла
        NULL,               // Командная строка
        NULL,               // Дескриптор процесса не наследуется
        NULL,               // Дескриптор потока не наследуется
        FALSE,              // Установка флага наследования в FALSE
        0,                  // Создание нового процесса
        NULL,               // Использование текущей директории
        NULL,               // Использование текущей среды
        &si,                // Атрибуты запускаемого процесса
        &pi                 // Информация о созданном процессе
    )) {
        // Процесс успешно запущен
        WaitForSingleObject(pi.hProcess, INFINITE); // Ожидание завершения процесса
        CloseHandle(pi.hProcess);
        CloseHandle(pi.hThread);
    }

    return 0;
}

#### 3. Критическая секция

Определение: Критическая секция в программировании представляет собой участок кода, который должен быть выполнен только одним потоком одновременно. Это обеспечивает защиту от одновременного доступа к общим ресурсам, таким как переменные или структуры данных, из разных потоков, предотвращая тем самым возможные конфликты и гонки данных.

Пример использования:

    #include <windows.h>
    
    // Объявление и инициализация критической секции
    CRITICAL_SECTION criticalSection;
    
    void someFunction() {
    // Вход в критическую секцию
    EnterCriticalSection(&criticalSection);
    
        // Критическая секция: безопасное выполнение операций
    
        // Выход из критической секции
        LeaveCriticalSection(&criticalSection);
    }
    
    int main() {
    // Инициализация критической секции
    InitializeCriticalSection(&criticalSection);
    
        // Здесь могут быть вызовы функций, работающих в разных потоках
    
        // Уничтожение критической секции
        DeleteCriticalSection(&criticalSection);
    
        return 0;
    }

#### 4. Семафор
Oпределение:
Семафор в программировании представляет собой средство синхронизации, позволяющее контролировать доступ к общим ресурсам в многозадачной среде.
Семафор содержит счетчик, который уменьшается при входе в критическую секцию и увеличивается при её выходе. Это позволяет ограничивать количество
потоков, которые могут одновременно выполнять критический участок кода.

Пример использования:
```
#include <windows.h>

// Объявление и инициализация семафора
HANDLE semaphore = CreateSemaphore(NULL, 1, 1, NULL);

void someFunction() {
    // Ожидание доступа к ресурсу (уменьшение счетчика семафора)
    WaitForSingleObject(semaphore, INFINITE);

    // Критическая секция: безопасное выполнение операций

    // Освобождение доступа к ресурсу (увеличение счетчика семафора)
    ReleaseSemaphore(semaphore, 1, NULL);
}

int main() {
    // Здесь могут быть вызовы функций, работающих в разных потоках

    // Закрытие семафора
    CloseHandle(semaphore);

    return 0;
}
```

#### 5. Процедурная декомпозиция:
Процедурная декомпозиция — это методология разработки программ, основанная на разбиении сложной задачи на более простые подзадачи, которые затем разрабатываются отдельно. Это применяется в процедурном программировании, где программа структурирована в виде процедур (функций или методов). Каждая процедура выполняет конкретную задачу, и комбинация этих процедур образует полную программу.

#### 6. Динамический полиморфизм:
Динамический полиморфизм — это концепция объектно-ориентированного программирования, которая позволяет использовать одинаковый интерфейс для работы с разными типами объектов. Это достигается с использованием механизмов, таких как виртуальные функции в C++ или интерфейсы и переопределение методов в других языках. Когда метод вызывается на объекте через указатель или ссылку на базовый класс, выполнение соответствующего метода определяется типом объекта во время выполнения.

#### 7. Инкапсуляция:
Инкапсуляция — это один из принципов объектно-ориентированного программирования, который предполагает упаковку данных и методов, работающих с этими данными, в единый компонент, называемый классом. Объекты этого класса имеют доступ только к публичным методам, что обеспечивает контроль доступа к данным. Инкапсуляция помогает скрыть реализацию и предоставлять интерфейс для взаимодействия с объектами, что способствует упрощению разработки, поддержки кода и сокрытию сложности от внешнего мира.

#### 8. Singleton
Описание через призму инкапсуляции:
Singleton — это порождающий паттерн, который гарантирует, что у класса есть только один экземпляр, и предоставляет глобальную
точку доступа к этому экземпляру. Он использует принцип инкапсуляции, чтобы скрыть создание экземпляра и предоставить точку доступа,
через которую клиенты могут получить этот экземпляр. Класс Singleton обычно содержит закрытое статическое поле для хранения экземпляра
и статический метод для получения этого экземпляра.

Пример использования на практике:
```
#include <iostream>

class Singleton {
private:
    static Singleton* instance; // Статическое поле для хранения единственного экземпляра
    Singleton() {} // Закрытый конструктор

public:
    static Singleton* getInstance() {
        if (!instance) {
            instance = new Singleton();
        }
        return instance;
    }

    // Другие методы и данные класса
};

int main() {
    Singleton* obj1 = Singleton::getInstance();
    Singleton* obj2 = Singleton::getInstance();

    // obj1 и obj2 указывают на один и тот же экземпляр Singleton
    // Проверка obj1 == obj2 вернет true
    return 0;
}
```

#### 9. State:
State — это поведенческий паттерн, который позволяет объекту изменять свое поведение при изменении его внутреннего
состояния. Он использует инкапсуляцию, чтобы представить каждое состояние объекта в виде отдельного класса,
а затем делегирует выполнение задач соответствующему классу состояния. Клиенты взаимодействуют с контекстом
объекта через общий интерфейс, не зная о деталях реализации конкретных состояний.

Пример использования на практике:
```
#include <iostream>

// Интерфейс для состояний
class State {
public:
    virtual void handle() = 0;
};

// Конкретное состояние A
class ConcreteStateA : public State {
public:
    void handle() override {
        std::cout << "Handling state A." << std::endl;
    }
};

// Конкретное состояние B
class ConcreteStateB : public State {
public:
    void handle() override {
        std::cout << "Handling state B." << std::endl;
    }
};

// Контекст, в котором происходит изменение состояний
class Context {
private:
    State* currentState;

public:
    Context(State* initialState) : currentState(initialState) {}

    void setState(State* newState) {
        currentState = newState;
    }

    void request() {
        currentState->handle();
    }
};

int main() {
    ConcreteStateA stateA;
    ConcreteStateB stateB;

    Context context(&stateA);

    // Переключение контекста на состояние B
    context.setState(&stateB);
    context.request();  // Обработка состояния B

    return 0;
}
```
В этом примере каждое состояние представлено отдельным классом, а объект Context делегирует выполнение задач соответствующим
классам состояний через общий интерфейс State.