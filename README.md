# telepot

**P**ython wrapper for [**Tele**gram B**ot** API](https://core.telegram.org/bots/api)

To use the Telegram Bot API, you first have to get a **bot account** by [chatting with the BotFather](https://core.telegram.org/bots).

He will then give you a **token**, something like `123456789:ABCdefGhIJKlmNoPQRsTUVwxyZ`. With the token in hand, you can start using telepot to access the bot account.

#### Installation

`pip install telepot` or `sudo pip install telepot` on Raspberry Pi

#### Test the account

```python
>>> import telepot
>>> bot = telepot.Bot('***** PUT YOUR TOKEN HERE *****')
>>> bot.getMe()
{u'username': u'YourBot', u'first_name': u'Your Bot', u'id': 123456789}
```

#### Receive messages

Try sending the bot a message. It gets the message by calling `getUpdates()`.

```python
>>> from pprint import pprint
>>> response = bot.getUpdates()
>>> pprint(response)
[{u'message': {u'chat': {u'first_name': u'Nick',
                         u'id': 999999999,
                         u'last_name': u'Lee'},
               u'date': 1436423831,
               u'from': {u'first_name': u'Nick',
                         u'id': 999999999,
                         u'last_name': u'Lee'},
               u'message_id': 106,
               u'text': u'Hi, how are you?'},
  u'update_id': 100000000}]
```

`999999999` is obviously a fake ID. `Nick` `Lee` is my real name, though.

Also note the `update_id`. It is an ever-increasing number. Next time you should use `getUpdates(offset=100000001)` to avoid getting the same old messages over and over.

```python
>>> bot.getUpdates(offset=100000001)
[]
```

#### An easier way to receive messages

It is kind of troublesome to keep checking messages. Fortunately, telepot can take care of the checking for you, and notify you whenever new messages arrive.

```python
>>> def handle_message(msg):
...     print 'Message from %s %s (ID: %d)' % (msg['from']['first_name'], msg['from']['last_name'], msg['from']['id'])
...     print 'Content: %s' % msg['text']
...
>>>  bot.notifyOnMessage(handle_message)
```

#### Send messages

Use a proper ID in place of `999999999`.

```python
>>> response = bot.sendMessage(999999999, 'I am fine')
>>> pprint(response)
{u'chat': {u'first_name': u'Nick', u'id': 999999999, u'last_name': u'Lee'},
 u'date': 1436425915,
 u'from': {u'first_name': u'Your Bot',
           u'id': 123456789,
           u'username': u'YourBot'},
 u'message_id': 114,
 u'text': u'I am fine'}
```

#### Send a custom keyboard

This is a great feature of the Telegram bot account! Try it.

```python
>>> show_keyboard = {'keyboard': [['Yes','No'], ['Maybe','Maybe not']]}
>>> bot.sendMessage(999999999, 'This is a custom keyboard', reply_markup=show_keyboard)
```

#### Hide the custom keyboard

```python
>>> hide_keyboard = {'hide_keyboard': True}
>>> bot.sendMessage(999999999, 'I am hiding it', reply_markup=hide_keyboard)
```

#### Send files

You may also send photos, videos, audios, documents, and stickers.

```python
>>> f = open('zzzzzzzz.png')  # this is some file on local disk
>>> response = bot.sendPhoto(999999999, f)
>>> pprint(response)
{u'chat': {u'first_name': u'Nick', u'id': 999999999, u'last_name': u'Lee'},
 u'date': 1436428640,
 u'from': {u'first_name': u'Your Bot',
           u'id': 123456789,
           u'username': u'YourBot'},
 u'message_id': 117,
 u'photo': [{u'file_id': u'APNpmPKVulsdkIFAILMDmhTAADmdcmdsdfaldalk',
             u'file_size': 2030,
             u'height': 64,
             u'width': 64},
            {u'file_id': u'VgbD_v___6AInvuOAlPldf',
             u'file_size': 7987,
             u'height': 0,
             u'width': 0}]}
```

Note that the server returns a number of `file_id`s, with various file sizes. These are thumbnails of the uploaded images. If you want to resend the same file, just give one of the `file_id`s.

```python
>>> bot.sendPhoto(999999999, 'APNpmPKVulsdkIFAILMDmhTAADmdcmdsdfaldalk')
```

#### Reference

Aside from `notifyOnMessage()`, all methods and parameters are straight mappings from the [Telegram Bot API](https://core.telegram.org/bots/api). I duplicate the list for your reference:

- [getMe](https://core.telegram.org/bots/api#getme)  
- [sendMessage](https://core.telegram.org/bots/api#sendmessage)  
- [forwardMessage](https://core.telegram.org/bots/api#forwardmessage)  
- [sendPhoto](https://core.telegram.org/bots/api#sendphoto)  
- [sendAudio](https://core.telegram.org/bots/api#sendaudio)  
- [sendDocument](https://core.telegram.org/bots/api#senddocument)  
- [sendSticker](https://core.telegram.org/bots/api#sendsticker)  
- [sendVideo](https://core.telegram.org/bots/api#sendvideo)  
- [sendLocation](https://core.telegram.org/bots/api#sendlocation)  
- [sendChatAction](https://core.telegram.org/bots/api#sendchataction)  
- [getUserProfilePhotos](https://core.telegram.org/bots/api#getuserprofilephotos)  
- [getUpdates](https://core.telegram.org/bots/api#getupdates)  
- [setWebhook](https://core.telegram.org/bots/api#setwebhook)  

As for `notifyOnMessage()`, please refer to the source code to discover its full power.
