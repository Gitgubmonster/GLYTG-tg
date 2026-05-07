# GLYT Bot

**GLYT** is a self-learning conversational bot that only repeats what you taught it.

You train it by arguing with it. Every message you send in reply is saved. Then the bot shuffles your own words and throws them back at you.

It doesn't understand anything. It doesn't learn meaning. It just memorizes and scrambles.

---

## How It Works

1. You reply to the bot
2. Bot saves your message to database
3. Bot picks a random message from the same chat
4. Bot shuffles all words in that message
5. Bot replies with the shuffled result

That's it. No AI. No logic. Just word salad.

---

## Features

- Chat-specific memory (each chat has its own database)
- Reply-triggered responses only
- SQLite storage (lightweight, no setup)
- Admin commands: export, import, clear
- Multi-language support (English/Russian)

---

## Commands

| Command | Description |
|---------|-------------|
| `/start` | Show welcome message |
| `/help` | Show help menu |
| `/credits` | Show creator info |
| `/stats` | Show how many messages stored |
| `/export` | Export chat database with a key |
| `/import <key>` | Import database from another chat |
| `/clear` | Clear all messages in this chat (admin) |
| `/lang` | Change language (English/Russian) |

---

## How to Deploy Your Own

### Requirements

- Python 3.7+
- Telegram bot token (from [@BotFather](https://t.me/BotFather))
- 5 minutes of free time

### Installation

```bash
pip install pyTelegramBotAPI
```

Minimal Bot Code Like GLYTG BOT

```python
import telebot
import sqlite3
import random

bot = telebot.TeleBot('YOUR_TOKEN')

conn = sqlite3.connect('glyt.db')
c = conn.cursor()
c.execute('CREATE TABLE IF NOT EXISTS messages (chat_id INTEGER, text TEXT)')

def save_message(chat_id, text):
    c.execute("INSERT INTO messages (chat_id, text) VALUES (?, ?)", (chat_id, text))
    conn.commit()

def get_random_message(chat_id):
    c.execute("SELECT text FROM messages WHERE chat_id = ? ORDER BY RANDOM() LIMIT 1", (chat_id,))
    row = c.fetchone()
    return row[0] if row else None

@bot.message_handler(func=lambda m: m.reply_to_message and m.reply_to_message.from_user.id == bot.get_me().id)
def handle_reply(message):
    save_message(message.chat.id, message.text)
    random_msg = get_random_message(message.chat.id)
    if random_msg:
        words = random_msg.split()
        random.shuffle(words)
        bot.reply_to(message, ' '.join(words))

bot.infinity_polling()
```

# Credits
Creator tg: @Im_ZnX

Creator git: https://github.com/Gitgubmonster
