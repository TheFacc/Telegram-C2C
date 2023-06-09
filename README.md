# Telegram C2C - Chat to Chat Forwarding Bot

This Telegram bot allows you to forward messages from multiple source chats to multiple destination chats (not strictly channels). It is built using Python and the Telethon library.

Features:
- **Filtering**: ignore message if it contains some specific word/string.

To do:
- Ability to retrieve message history since last run (currently it forwards only messages received while it's running)
- Make an actual bot to manage its functionality and edit its config on the go

_**NOTE (important if you didn't know this was possible!)**: technically it's not really a Telegram bot. It's kind of a headless bot. It runs on your computer and does stuff, but does not link to an actual bot created with BotFather. It simply uses your Telegram session, so it behaves as a user. This is possible with **[Telethon](https://github.com/LonamiWebs/Telethon)**. This is the only way to manage channels like this, since you cannot add a bot to a channel unless you're an admin of the source channel._

## Prerequisites

- Python 3.6+
- [Telethon](https://github.com/LonamiWebs/Telethon) library

## Usage

1. Clone the repository:

   ```bash
   git clone https://github.com/TheFacc/Telegram-C2C.git
   ```

2. Install the required dependencies:

   ```bash
   pip install telethon
   ```

3. Create a `config.py` file and add your Telegram API credentials (these are just fake examples, use your own!). Get API/Hash strings from [here](https://my.telegram.org/) and numeric IDs of source/target chats from [@getidsbot](t.me/getidsbot). An ID can also be the handle/link/etc, as shown below or in [Telethon documentation](https://docs.telethon.dev/en/stable/concepts/entities.html#getting-entities).:

   ```python
   API_ID = YOUR_API_ID # '1234567'
   API_HASH = YOUR_API_HASH # 'd6s7687nm7hf5ndgb6d8ssv7dg'
   CHAT_IDS = [
     [-100123333, -100123334, 'source1', -100123456] # source group 1
     [-100123789, 'telegram.me/+v12a34b56c'] # source group 2
   ]
   TARGET_IDS = [
     'mychannel1' # destination chat 1
     'mychannel2' # destination chat 2
   ]
   # Ignore messages containing any of these words:
   # - exclude globally
   EXCLUDED_WORDS = ['t.me','joinchat','giveaway','trading','🔥']
   # - exclude per-group (list of lists, one per source group)
   EXCLUDED_WORDS_GROUP = [
    ['group1 badword', 'another'],
    [], # can be empty if you just want to ignore globals
   ]
   ```

4. Run the bot:

   ```bash
   python3 Telegram_C2C.py
   ```

5. It will ask for either your phone number or bot token. You have to enter your phone number in order for the bot to work (e.g. `+393456789000`), and confirm the code you receive. From this moment, the bot will store your session and behave as your user, based on the python code in this repo. Enjoy!

## Note for Nix users

I'm a NixOS user. If you know you know. Here's a Nix shell tailored to run this bot, just run `nix-shell shell_c2c.nix`:
```nix
# shell_c2c.nix
{ pkgs ? import <nixpkgs> {} }:

let
  c2c-python-packages = python-packages: with python-packages; [
    pip
    python-telegram-bot
    telethon
  ];
  python-c2c = pkgs.python310.withPackages c2c-python-packages;

in

  pkgs.mkShell {
    nativeBuildInputs = [
      python-c2c
    ];
}
```

## Contributing

Contributions are welcome! If you find any issues or have suggestions for improvement, please open an issue, submit a pull request, or [contact me](https://thefacc.github.io/).

## License

This project is licensed under the MIT License.