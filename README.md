# Installation \& Setup on a raspberry pi

Don't just copy and paste everything! Step 3, 6 and 9 require interaction. The rest can just pasted
in.

```
#  1. Set up pyenv (you need python 3.5... otherwise you'll need to update the dependencies)
sudo apt-get install -y bzip2 libbz2-dev libreadline6 libreadline6-dev libffi-dev \ 
  libssl1.0-dev sqlite3 libsqlite3-dev
curl https://pyenv.run | bash
echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> $HOME/.bashrc
echo 'eval "$(pyenv init -)"' >> $HOME/.bashrc
echo 'eval "$(pyenv virtualenv-init -)"' >> $HOME/.bashrc
exec "$SHELL"
pyenv install 3.5.7
#  2. Clone this repo
git clone https://github.com/samuelsmal/telegram-twitter-forwarder-bot.git
cd telegram-twitter-forwarder-bot
#  3. Adapt envs (check secrets.env.tmpl, store without the .tmpl ending)
#  4. Setup python env
pyenv virtualenv 3.5.7 teltwitbot
pyenv local teltwitbot
pip install -r requirements.txt
#  5. Adapt python path
which python
#  6. Take output from above and modify the service file `teltwitbot.service`
#     You'll need to adapt more. Some other paths as well.
#     Note that systemd expects absolute paths.
#  7. Move service file
sudo cp ./teltwitbot.service /etc/systemd/system/
#  8. Start service
sudo systemctl daemon-reload
sudo systemctl start teltwitbot
#  9. Check output
journalctl -u teltwitbot
# 10. If good:
sudo systemctl enable teltwitbot
# 11. Reboot and check again
# 12. Enjoy!
```

# Below is the original README

# telegram-twitter-forwarder-bot
![logo](logo/logo.png)

Hello! This projects aims to make a [Telegram](https://telegram.org) bot that forwards [Twitter](https://twitter.com/) updates to people, groups, channels, or whatever Telegram comes up with!

You can check it on Telegram: [@TwitterForwarderBot](https://telegram.me/TwitterForwarderBot)

#### Credit where credit is due

This is based on former work:
- [python-telegram-bot](https://github.com/leandrotoledo/python-telegram-bot)
- [tweepy](https://github.com/tweepy/tweepy)
- [peewee](https://github.com/coleifer/peewee)
- [envparse](https://github.com/rconradharris/envparse)
- also, python, pip, the internets, and so on


So, big thanks to anyone who contributed on these projects! :D

#### How do I run this?

**The code is currently targeting Python 3.5**
```
# clone this thing
# create your virtualenv, activate it, etc
# virtualenv -p python3 venv
# . venv/bin/activate
pip install -r requirements.txt
source secrets.env
python main.py
```

#### secrets.env?? u wot m8?

First, you'll need a Telegram Bot Token, you can get it via BotFather ([more info here](https://core.telegram.org/bots)).

Also, setting this up will need an Application-only authentication token from Twitter ([more info here](https://dev.twitter.com/oauth/application-only)). Optionally, you can provide a user access token and secret.

You can get this by creating a Twitter App [here](https://apps.twitter.com/).

Bear in mind that if you don't have added a mobile phone to your Twitter account you'll get this:

>You must add your mobile phone to your Twitter profile before creating an application. Please read https://support.twitter.com/articles/110250-adding-your-mobile-number-to-your-account-via-web for more information.

Get a consumer key, consumer secret, access token and access token secret (the latter two are optional), fill in your `secrets.env` and then run the bot!
