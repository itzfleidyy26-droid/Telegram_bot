import telebot
import random
import os
from flask import Flask
import threading
import time

# --- НАСТРОЙКИ ---
TOKEN = os.environ.get("TOKEN")
bot = telebot.TeleBot(TOKEN)

# --- КОСТЫЛЬ ДЛЯ RENDER (HTTP-СЕРВЕР) ---
app = Flask(__name__)

@app.route('/')
def index():
    return "Bot is running, pidr."

def run_http():
    port = int(os.environ.get('PORT', 10000))
    app.run(host='0.0.0.0', port=port)

threading.Thread(target=run_http).start()

# --- БАЗА ДАННЫХ (простые списки) ---
INSULTS = [
    "Слышь, {name}, ты чё, с дуба рухнул?",
    "Иди нахуй со своим вопросом, петух.",
    "Твоя мамка сегодня по акции, что ли?",
    "Ты ебанулся? Такие вопросы задавать — это надо быть долбоёбом.",
    "Слышь, да ты вообще кто по жизни, гнида?",
    "У тебя мозги есть? Или ты только жрать умеешь?",
    "Рот закрой, а то проветрю.",
    "Твои вопросы — как твоё лицо: кривые и бесполезные.",
    "Я бы тебя послал, но ты уже там живёшь.",
    "Ты чё, обезьяна, клавиши нажимаешь?"
]

MAT_PHRASES = [
    "Иди нахуй, {name}",
    "Ты долбоёб, {name}",
    "Отъебись, {name}",
    "Ты гнида кусок, {name}",
    "Соси хуй, {name}",
    "Ты пидор гнойный, {name}",
    "Иди в жопу, {name}",
    "Ты петух, {name}"
]

JOKES = [
    "Идёт мужик по лесу, видит — хуй стоит. Подходит, а это не хуй, а ... (дальше сам придумай, я устал)",
    "Твоя мамка настолько толстая, что когда садится в такси, стрелка цен меняется на 'фура'.",
    "Встречаются два друга: — Ты где работаешь? — На стройке. — А кем? — Долбоёбом. — Это как? — Прихожу, спрашиваю: 'Чё делать?', мне говорят: 'Иди нахуй'. Вот иду...",
    "Твой интеллект как моя зарплата — ниже плинтуса.",
    "Ты такой тупой, что даже клавиатура плачет, когда ты печатаешь."
]

WHOIS_INSULTS = [
    "Этот пидор {name} — конченый долбоёб, который даже не умеет пользоваться ботом.",
    "{name}? А, это тот мудак, который думает, что он умный. Спойлер: нет.",
    "{name} — ходячее доказательство того, что эволюция может ошибаться.",
    "{name} настолько тупой, что даже Google не может найти его мозги.",
    "{name} — человек, который делает селфи в общественном туалете и думает, что это искусство."
]

RATE_COMMENTS = [
    "Оценка: {rate}/10. Как внешность, так и мозги.",
    "{rate}/10. Даже ЕГЭ сдал бы лучше, чем ты выглядишь.",
    "{rate} баллов из 10. Твоя мамка расстроена.",
    "{rate}/10. И это ещё с натяжкой, как резинка на трусах.",
    "Ставлю {rate}/10. Потолок, выше не прыгнешь."
]

DURAK_PHRASES = [
    "{name} — долбоёб",
    "{name} — петух",
    "{name} — гнида",
    "{name} — уёбок",
    "{name} — мудак",
    "{name} — пидор",
    "{name} — ебанат",
    "{name} — дегенерат"
]

KTO_VARIANTS = [
    "Ты сегодня петух",
    "Ты сегодня гнида",
    "Ты сегодня уёбок",
    "Ты сегодня мудак",
    "Ты сегодня пидор",
    "Ты сегодня ебанат",
    "Ты сегодня дебил"
]

ABUSE_TEMPLATES = [
    "Я твою мать {action}, {name}, чтоб ты {result}!",
    "Слышь, {name}, иди {action}, пока я не {result} тебя!",
    "Ты {name} — {insult}, даже не мечтай {result}.",
    "Пошёл ты {action}, {name}, у тебя {result} вместо мозгов."
]

ABUSE_ACTIONS = ["имел", "трахал", "выебал", "нагнул", "отодрал"]
ABUSE_RESULTS = ["сдох", "провалился", "утонул", "сломался", "сгорел"]
ABUSE_INSULTS = ["дебил", "дегенерат", "идиот", "кретин", "имбецил"]

TOPICS = {
    "программисты": [
        "Программисты — это такие уёбки, которые пишут код и думают, что они боги.",
        "Самый страшный сон программиста — это баг, который нельзя воспроизвести.",
        "Программист без бубна — не программист, а так, пидор гнойный."
    ],
    "менеджеры": [
        "Менеджеры — это люди, которые не умеют ничего, кроме как дрочить мозги.",
        "Менеджер без плана — как петух без хуя.",
        "Самый опасный вид менеджера — тот, который вчера был стажёром."
    ],
    "дизайнеры": [
        "Дизайнеры — это художники, которые не умеют рисовать, но умеют включать фотошоп.",
        "Дизайнер без макета — как я без мата.",
        "Лучший дизайн — это когда хуй поймёшь, где кнопка."
    ],
    "начальники": [
        "Начальник — это человек, который не умеет делать работу, но умеет её заёбывать.",
        "Если начальник говорит 'сделайте красиво', значит, он сам не знает, чего хочет.",
        "Начальник без секретарши — как член без яиц."
    ]
}

FACTS = [
    "Знаешь ли ты, что твой IQ равен комнатной температуре?",
    "Факт: 99.9% людей умнее тебя. Остальные 0.1% — это твои родственники.",
    "Научно доказано: ты — говно.",
    "Исследования показывают, что ты тупее, чем кажешься. А ты кажешься очень тупым.",
    "Факт: ты прочитал это сообщение и ничего не понял."
]

MAMA_JOKES = [
    "У {name} мама настолько старая, что помнит, когда я был человеком.",
    "Мама {name} настолько толстая, что когда садится на диету, ей нужно бронировать столик заранее.",
    "Мама {name} настолько волосатая, что когда она в душе, кажется, что там мочалка разговаривает.",
    "Мама {name} настолько глупая, что думает, что 'анонимный' — это вид инфекции.",
    "Мама {name} настолько бедная, что когда я кидаю ей ссылку на скидки, она думает, что это порно."
]

# --- ФЛАГ ДЛЯ СЕРЬЁЗНОГО РЕЖИМА ---
serious_mode = False

# --- КОМАНДЫ ---

@bot.message_handler(commands=['start'])
def send_welcome(message):
    bot.reply_to(message, f"О, ещё один долбоёб. Чё надо, {message.from_user.first_name}?")

@bot.message_handler(commands=['help'])
def send_help(message):
    help_text = (
        "Пиши команды, петух:\n"
        "/pzdc [текст] - перевод всего в мат\n"
        "/mat [число] - генератор мата\n"
        "/whois [юзер] - оскорбительная характеристика\n"
        "/rate [юзер] - оценка по 10-балльной шкале\n"
        "/joke - грязная шутка\n"
        "/durak [юзер] - случайное оскорбление\n"
        "/kto - кто ты сегодня?\n"
        "/abuse [юзер] - длинное оскорбление\n"
        "/say [текст] - повторялка с матом\n"
        "/gen [тема] - генерация по теме\n"
        "/battle [юзер1] [юзер2] - кто больший уёбок\n"
        "/fact - случайный факт\n"
        "/mama [юзер] - шутка про маму\n"
        "/helpme - если реально плохо (серьёзно)\n"
        "/stop - переключиться в вежливый режим"
    )
    bot.reply_to(message, help_text)

@bot.message_handler(commands=['pzdc'])
def pzdc(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Извините, сейчас я в вежливом режиме. Напишите /helpme для возврата.")
        return
    text = message.text.replace('/pzdc', '').strip()
    if not text:
        text = "ты сам не знаешь, что хочешь"
    bot.reply_to(message, f"Пойди нахуй со своим '{text}', петух!")

@bot.message_handler(commands=['mat'])
def mat(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим включён. Без мата.")
        return
    args = message.text.split()
    count = 1
    if len(args) > 1 and args[1].isdigit():
        count = int(args[1])
    name = message.from_user.first_name or "Неизвестный"
    response = "\n".join([random.choice(MAT_PHRASES).format(name=name) for _ in range(count)])
    bot.reply_to(message, response)

@bot.message_handler(commands=['whois'])
def whois(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: кто угодно, но вы мне нравитесь.")
        return
    args = message.text.split(maxsplit=1)
    target = args[1] if len(args) > 1 else "ты"
    insult = random.choice(WHOIS_INSULTS).format(name=target)
    bot.reply_to(message, insult)

@bot.message_handler(commands=['rate'])
def rate(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: вы заслуживаете 10/10.")
        return
    args = message.text.split(maxsplit=1)
    target = args[1] if len(args) > 1 else "ты"
    rate = random.randint(1, 10)
    comment = random.choice(RATE_COMMENTS).format(rate=rate)
    bot.reply_to(message, f"{target}: {comment}")

@bot.message_handler(commands=['joke'])
def joke(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: анекдот про кота.")
        return
    bot.reply_to(message, random.choice(JOKES))

@bot.message_handler(commands=['durak'])
def durak(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: вы не дурак, вы хороший.")
        return
    args = message.text.split(maxsplit=1)
    target = args[1] if len(args) > 1 else message.from_user.first_name
    phrase = random.choice(DURAK_PHRASES).format(name=target)
    bot.reply_to(message, phrase)

@bot.message_handler(commands=['kto'])
def kto(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: вы прекрасны.")
        return
    bot.reply_to(message, random.choice(KTO_VARIANTS))

@bot.message_handler(commands=['abuse'])
def abuse(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: вы хороший человек.")
        return
    args = message.text.split(maxsplit=1)
    target = args[1] if len(args) > 1 else message.from_user.first_name
    action = random.choice(ABUSE_ACTIONS)
    result = random.choice(ABUSE_RESULTS)
    insult = random.choice(ABUSE_INSULTS)
    template = random.choice(ABUSE_TEMPLATES)
    response = template.format(action=action, name=target, result=result, insult=insult)
    bot.reply_to(message, response)

@bot.message_handler(commands=['say'])
def say(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: повторяю вежливо...")
        return
    text = message.text.replace('/say', '').strip()
    if not text:
        text = "ничего не сказал"
    bot.reply_to(message, f"Слышь, ты сказал: '{text}'. Ну и хуйня, петух.")

@bot.message_handler(commands=['gen'])
def gen(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: генерация отключена.")
        return
    args = message.text.split(maxsplit=1)
    topic = args[1].lower().strip() if len(args) > 1 else "программисты"
    if topic in TOPICS:
        response = random.choice(TOPICS[topic])
    else:
        response = f"Тема '{topic}'? Даже я не знаю, что это за хуйня. Попробуй: программисты, менеджеры, дизайнеры, начальники."
    bot.reply_to(message, response)

@bot.message_handler(commands=['battle'])
def battle(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: вы оба хороши.")
        return
    args = message.text.split()
    if len(args) >= 3:
        user1 = args[1]
        user2 = args[2]
    else:
        user1 = message.from_user.first_name
        user2 = "сосед"
    winner = random.choice([user1, user2])
    bot.reply_to(message, f"В битве уёбков побеждает {winner}! Поздравляю, хуйло.")

@bot.message_handler(commands=['fact'])
def fact(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: факт — вы замечательный.")
        return
    bot.reply_to(message, random.choice(FACTS))

@bot.message_handler(commands=['mama'])
def mama(message):
    global serious_mode
    if serious_mode:
        bot.reply_to(message, "Вежливый режим: ваша мама замечательная женщина.")
        return
    args = message.text.split(maxsplit=1)
    target = args[1] if len(args) > 1 else "твоя"
    joke = random.choice(MAMA_JOKES).format(name=target)
    bot.reply_to(message, joke)

@bot.message_handler(commands=['helpme'])
def helpme(message):
    global serious_mode
    serious_mode = False
    bot.reply_to(message, 
                 "Слышь, если реально плохо — держи контакты, где помогут:\n"
                 "8-800-555-35-35 — бесплатно, анонимно, 24/7\n"
                 "Или напиши сюда: https://psi-help.ru\n"
                 "Не будь петухом, живи.")

@bot.message_handler(commands=['stop'])
def stop(message):
    global serious_mode
    serious_mode = True
    bot.reply_to(message, "Ок, переключаюсь в вежливый режим. Буду общаться без мата. Если что, /helpme вернёт обратно.")

# --- ОБРАБОТКА ОБЫЧНЫХ СООБЩЕНИЙ ---
@bot.message_handler(func=lambda message: True)
def handle_message(message):
    global serious_mode
    name = message.from_user.first_name or "Неизвестный"

    # Проверка на серьёзные темы
    text = message.text.lower()
    serious_keywords = ["суицид", "убью себя", "жить не хочу", "помоги", "плохо", "тоска", "депрессия"]
    for word in serious_keywords:
        if word in text:
            bot.reply_to(message, 
                         f"Слышь, {name}, это не шутки. Держи номер: 8-800-555-35-35. "
                         "Если реально хреново — позвони. Живи, падла.")
            return

    if serious_mode:
        bot.reply_to(message, f"Вежливый режим: привет, {name}. Чем могу помочь? (Напишите /help для команд)")
        return

    # Обычный троллинг
    insult = random.choice(INSULTS).format(name=name)
    bot.reply_to(message, insult)

# --- ЗАПУСК ---
print("Бот запущен, ебать его в качель...")
bot.polling(none_stop=True)
