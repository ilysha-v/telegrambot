# scala-telegram-bot
[![Build Status](https://magnum.travis-ci.com/hzengin/telegrambot.svg?token=Eu31bmPEzUsSvufqwvjh&branch=master)](https://magnum.travis-ci.com/hzengin/telegrambot)
"Batteries Included" Telegram Bot API wrapper for Scala



## Features
- Fully asynchronous
- [spray.io](spray.io) powered.
- Declerative DSL for simple bot features
- Strongly-typed

## Supported Methods
- getMe
- sendMessage
- getUpdates
- forwardMessage
- sendPhoto
- sendAudio
- sendVoice (coming soon)
- sendDocument
- sendSticker
- sendVideo (coming soon)
- sendLocation
- sendChatAction
- getUserProfilePhotos
- getUpdates
- Custom keyboard markups
- Webhook with a SSL reverse proxy

## TODO
 - Built-in Webhook support
 - Better error handling
 - Documentation

## Configuration
Check [reference.conf](https://github.com/hzengin/telegrambot/blob/master/src/main/resources/reference.conf) for configuration.

## Installation
There is no release yet; project still lack too much improvement. Wait for updates.

## Usage
#### Simple commands + simple answers
```scala
object GreeterBot extends TelegramBot with Polling with Declerative {
  on("/start") { implicit message: Message =>
    reply("Welcome")
  }
}
```
#### Using received message
```scala
object GreeterBot extends TelegramBot with Polling with Declerative {
  on("/start") { implicit message: Message =>
    message.from match {
      case Some(user) => reply("Welcome, " + user.firstName)
      case _ =>
    }
  }
}
```
#### Sending photos
```scala
object GreeterBot extends TelegramBot with Polling with Declerative {
  on("/start") { implicit message: Message =>
    sendPhoto("~/image.jpg")
  }
}
```

#### When You Need More Power
```scala
object GreeterBot extends TelegramBot with Polling with Declerative {
  when { message =>
    message.photo match {
      case Some(photo) => true
      case None => false
    }
  } perform { implicit message =>
    reply("Nice Smile!")
  }
}
```
#### "Cuckoo Clock?" Why Not?
```scala
object GreeterBot extends TelegramBot with Polling with Declerative {
  every(1 hours) {
    send("Ping!", 31415926535)
  }
}
```

#### Sending Messages Not Always Successful
```scala
object TestBot extends TelegramBot with Polling with Declerative {
  sendTo("test", 1093654812) map {
    case Left(message) =>
      println(message);
    case Right(error) if error.code == 403 =>
      println("No, we are not allowed to send messages to this chat");
    case Right(error) if error.code == 400 =>
      println("No, chat doesn't exists");
  }
}
```
