# Config Language Parser

## Описание проекта

`Config Language Parser` — это инструмент для парсинга конфигурационного языка, который позволяет преобразовывать входные текстовые конфигурации в JSON. Он поддерживает обработку констант, массивов, вложенных словарей (table) и вычисление значений через ссылки на константы.

Этот проект идеально подходит для обработки сложных конфигураций, представленных в текстовом виде, с дальнейшей интеграцией в системы, которые работают с JSON.

---

## Основной функционал

1. **Поддерживаемые структуры:**
   - **Константы**: Строковые или числовые значения, задаются через `const`.
   - **Ссылки на константы**: Используются через `|name|` для вставки значений констант.
   - **Массивы**: JSON-совместимые списки.
   - **Словари (table)**: Вложенные структуры ключ-значение, поддерживают многослойную вложенность.

2. **Ключевые функции:**
   - Преобразование текста конфигурации в JSON.
   - Обработка сложных вложенных структур.
   - CLI-интерфейс для удобной работы через командную строку.

---

## Установка

1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/yourusername/config-parser.git
   cd config-parser
   ```

2. Убедитесь, что установлен Python 3.9 или выше:
   ```bash
   python --version
   ```

3. Установите зависимости:
   В данном проекте используются только стандартные библиотеки Python, поэтому дополнительных зависимостей нет.

---

## Использование

1. **Команда для запуска:**
   ```bash
   python config_parser.py -i input_config1.txt -o output.json
   ```

2. **Аргументы:**
   - `-i` или `--input`: Путь к входному текстовому файлу конфигурации.
   - `-o` или `--output`: Путь для сохранения сгенерированного JSON.

3. **Пример вызова:**
   Для файла `input_config1.txt` команда:
   ```bash
   python config_parser.py -i input_config1.txt -o output.json
   ```
   Сохранит результат в `output.json`.

---

## Формат входных данных

Пример файла конфигурации `input_config1.txt`:
```plaintext
const db_name = "main_db";
const max_connections = 100;

// Описание базы данных
table(
    name => |db_name|,
    settings => table(
        connections => |max_connections|,
        timeout => 30
    )
)
```

---

## Формат выходных данных

Пример выходного JSON (`output.json`):
```json
[
    {
        "name": "main_db",
        "settings": {
            "connections": 100,
            "timeout": 30
        }
    }
]
```

---

## Примеры работы

1. **Входной файл `input_config1.txt`**:
   ```plaintext
   const db_name = "main_db";
   const max_connections = 100;

   table(
       name => |db_name|,
       settings => table(
           connections => |max_connections|,
           timeout => 30
       )
   )
   ```

   **Команда**:
   ```bash
   python config_parser.py -i input_config1.txt -o output.json
   ```

   **Результат**:
   ```json
   [
       {
           "name": "main_db",
           "settings": {
               "connections": 100,
               "timeout": 30
           }
       }
   ]
   ```

2. **Входной файл `input_config2.txt`**:
   ```plaintext
   const network_name = "LAN";
   const devices = ["Router", "Switch", "PC"];

   table(
       name => |network_name|,
       devices => |devices|
   )
   ```

   **Команда**:
   ```bash
   python config_parser.py -i input_config2.txt -o output2.json
   ```

   **Результат**:
   ```json
   [
       {
           "name": "LAN",
           "devices": ["Router", "Switch", "PC"]
       }
   ]
   ```

---

## Тестирование

Пример теста для проверки работы парсера:
```python
import unittest
from config_parser import ConfigParser

class TestConfigParser(unittest.TestCase):
    def test_parse_basic(self):
        text = """
        const key = "value";
        table(
            name => |key|,
            settings => table(
                timeout => 30
            )
        )
        """
        parser = ConfigParser()
        result = parser.parse(text)
        expected = [
            {
                "name": "value",
                "settings": {
                    "timeout": 30
                }
            }
        ]
        self.assertEqual(result, expected)

if __name__ == '__main__':
    unittest.main()
```

Запустите тесты:
```bash
python -m unittest
```

---

## Примечания

- **Особенности**:
  - Обрабатывает вложенные структуры и константы.
  - Поддерживает комментарии (`//` или `*`).

- **Ограничения**:
  - Парсер ожидает строго структурированные входные данные.
  - В случае ошибок парсинга генерируются исключения с подробным описанием.

---

## Автор

Создан для демонстрации парсинга кастомного конфигурационного языка с генерацией JSON. Для предложений и доработок обратитесь через репозиторий.