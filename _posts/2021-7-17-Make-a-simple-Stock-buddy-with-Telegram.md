---
layout: post
title: How to make a simple Stock buddy with Telegram! (part 1)
---

![]({{ site.baseurl }}/images/stock.jpg)

Stock market is changing dramatically day by day, minute by minute. As amateur players of the market, we, however, may not want to sit down in front of the board all day long, checking for the tickers jumping up and down, continuously from red to green, then from green to red again and again.

This actually brought me a question, ***Why don't we build a small app to inform us at important moments?*** That would help us stay up-to-date without spending too much time on this tedious job.

## Let's try that idea with Telegram

Telegram is free and open. This theme is also reflected in its [Bot API and platform](https://core.telegram.org/bots/api).

Based on this core concept of Telegram platform, we are going to build a Telegram bot that sends messages to a group chat.

To start we first need to have a Telegram account beforehand.

On the search box, find `@botfather` - *"one bot to rule them all"*.

![]({{ site.baseurl }}/images/telegrammer/bot_father.png)

To create a new bot, send the command `/newbot` and put in the bot's name and username that we want. It's pretty simple, huh!

![]({{ site.baseurl }}/images/telegrammer/bot_creation.png)

After the creation is completed, BotFather will give us a Token, let's save this for our program.

![]({{ site.baseurl }}/images/telegrammer/bot_creation_complete.png)

In order to enable bot sending message to the group chat, it needs to update the group privacy for the bot.

![]({{ site.baseurl }}/images/telegrammer/bot_setting_1.png)
![]({{ site.baseurl }}/images/telegrammer/bot_setting_2.png)
![]({{ site.baseurl }}/images/telegrammer/bot_setting_3.png)

Next, the remaining thing we have to do on Telegram app is to create a group chat in which our newly created bot will message to. After that, add the bot to the group and give it the admin permission.

Once the bot is in the room and the privacy mode is disabled for the bot, it is ready to write our first program, `send_message.py`, to ask the bot send message to our group.

```python
import requests
from credentials import TOKEN, GROUP_CHAT_ID


def format_message(message: str) -> str:
    return message.replace(" ", "%20")


message = "hello world!"
message = format_message(message)
print(message)
request_body = f"""https://api.telegram.org/bot{TOKEN}/sendMessage?chat_id={GROUP_CHAT_ID}&text={message}"""
response = requests.post(request_body)
```
The program uses simple `requests` package to send message via POST request to telegram api.

In this piece of code, there are two variables that we would be interested in, `TOKEN` and `GROUP_CHAT_ID`. `TOKEN` is what we received from BotFather above, while to get `GROUP_CHAT_ID`, we could send a POST request to `https://api.telegram.org/bot<TOKEN>/getUpdates`. Using Postman, we got what we look for as follows.

![]({{ site.baseurl }}/images/telegrammer/get_group_chat_id.png)

After finding the credentials, pass them to our program and run the python command

```
python send_message.py
```

And Dinggg!! The first message from our Bot is comming in.

![]({{ site.baseurl }}/images/telegrammer/final.png)
