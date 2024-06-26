Документація для Чат-бота

Опис проекту

Цей проект реалізує чат-бота, який підтримує різні функції, такі як перегляд погоди, встановлення і перегляд налаштувань користувача, ведення історії чату та сповіщення через електронну пошту. Бот реалізований з використанням патернів проектування Strategy, State та Observer для підвищення гнучкості та розширюваності.

Структура проекту

Патерни проектування

Strategy: Дозволяє визначати різні алгоритми обробки повідомлень та перемикатися між ними.

State: Дозволяє змінювати поведінку бота в залежності від його стану.

Observer: Дозволяє іншим об'єктам спостерігати та реагувати на зміни в боті.

Компоненти

TextProcessingStrategy
Абстрактний клас для визначення стратегії обробки тексту.

process(message: str, user_id: str = "default_user") -> str: Метод для обробки повідомлень.
BotState

Абстрактний клас для визначення стану бота.

handle_message(bot, message: str, user_id: str = "default_user"): Метод для обробки повідомлень залежно від стану.

Observer
Абстрактний клас для визначення спостерігача.

update(message: str, response: str): Метод для оновлення спостерігачів.
Subject
Абстрактний клас для визначення суб'єкта.

attach(observer: Observer): Метод для додавання спостерігача.
detach(observer: Observer): Метод для видалення спостерігача.
notify(message: str, response: str): Метод для сповіщення спостерігачів.
Реалізації
Стани бота
InitialState: Початковий стан бота, який використовує стратегію привітання.
WaitingForInputState: Стан, який очікує вводу від користувача.
Стратегії обробки тексту
GreetingStrategy: Стратегія для привітання користувача.
WeatherStrategy: Стратегія для отримання погоди за допомогою API OpenWeatherMap.
Спостерігачі
Logger: Логування повідомлень та відповідей у файл.
DataSaver: Збереження повідомлень та відповідей в SQLite базу даних.
EmailNotifier: Надсилання повідомлень на електронну пошту.
SMSNotifier: Надсилання повідомлень через SMS за допомогою Twilio (коментовано).
Додаткові класи
ChatHistory: Управління історією чату.
UserSettings: Управління налаштуваннями користувача.
Основний клас
Bot
set_state(state: BotState): Встановлює поточний стан бота.
set_strategy(strategy: TextProcessingStrategy): Встановлює поточну стратегію обробки тексту.
attach(observer: Observer): Додає спостерігача.
detach(observer: Observer): Видаляє спостерігача.
notify(message: str, response: str): Сповіщає спостерігачів про нове повідомлення.
process_message(message: str, user_id: str = "default_user") -> str: Обробляє повідомлення за допомогою поточної стратегії.
handle_message(message: str, user_id: str = "default_user") -> str: Обробляє повідомлення в залежності від стану та команд.
Інтерфейс командного рядка
CommandLineInterface
format_response(response: str) -> str: Форматує відповідь з поточним часом.
show_history(): Показує історію чату.
display_greeting(): Відображає привітання при запуску.
start(): Запускає інтерфейс командного рядка для взаємодії з ботом.
load_history(): Завантажує історію чату з бази даних.
Використання
Запуск бота
Імпортуйте необхідні бібліотеки та класи.
Створіть екземпляр класу Bot.
Додайте спостерігачів до бота (логгер, зберігач даних, сповіщувач).
Створіть екземпляр CommandLineInterface з ботом.
Запустіть CLI за допомогою методу start().
python
Копіювати код
# Створення чат-бота та додавання спостерігачів
bot = Bot()
logger = Logger()
data_saver = DataSaver("chatbot.db")
email_notifier = EmailNotifier(
    smtp_server="smtp.yourserver.com", smtp_port=587, 
    login="yourlogin", password="yourpassword", 
    from_email="you@example.com", to_email="user@example.com"
)
# sms_notifier = SMSNotifier(
#     account_sid="your_twilio_account_sid", 
#     auth_token="your_twilio_auth_token", 
#     from_number="your_twilio_number", 
#     to_number="destination_number"
# )

bot.attach(logger)
bot.attach(data_saver)
bot.attach(email_notifier)
# bot.attach(sms_notifier)

# Запуск інтерфейсу командного рядка
cli = CommandLineInterface(bot)
cli.start()
Команди
exit: Завершити роботу бота.
history: Показати історію чату.
set city [city_name]: Встановити місто за замовчуванням.
get city: Отримати місто за замовчуванням.
clear history: Очистити історію чату.
get history: Отримати історію чату.
Приклади використання
Встановлення міста:
vbnet
Копіювати код
You: set city Kyiv
ChatBot: [15:00:00] Default city set to Kyiv
Отримання міста:
vbnet
Копіювати код
You: get city
ChatBot: [15:01:00] Your default city is Kyiv
Отримання погоди:
vbnet
Копіювати код
You: What's the weather in Kyiv?
ChatBot: [15:02:00] The weather in Kyiv is clear sky with a temperature of 25°C.
Очистка історії:
bash
Копіювати код
You: clear history
ChatBot: [15:03:00] Chat history cleared.
Отримання історії:
vbnet
Копіювати код
You: get history
ChatBot: [15:04:00] 
2024-06-23 15:00:00 - You: set city Kyiv - Bot: Default city set to Kyiv
2024-06-23 15:01:00 - You: get city - Bot: Your default city is Kyiv
Ця документація охоплює основні аспекти проекту та використання чат-бота. Ви можете розширити функціональність, додаючи нові стратегії, стани та спостерігачів за потреби.
