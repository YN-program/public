---
title: 「django-axes」 でログイン試行回数を制限する
date: 2022-03-27
categories:
  - "programming"
tags:
  - "django"
menu:
main:
  name: プログラミング
  weight: 2
---


Webサイトにて、不正ログインを防ぐための django-axes。
ブルートフォース攻撃やパスワードリスト攻撃、辞書攻撃などの対策になる。
<!--more-->

ブルートフォース攻撃（総当たり攻撃）：パスワードや暗証番号に対して、可能性のある全ての組み合わせを総当たりで試す方法。

パスワードリスト攻撃：流出したIDとパスワードをリストで管理し、リスト上のIDとパスワードを用いて標的システムへの侵入を試みる攻撃。異なるサービスで同じIDとパスワードを使用していた場合、パスワードリスト攻撃によりシステムへの侵入を許すことになる。

辞書攻撃：辞書に掲載されているような語、一般的に良く知られている単語を用いてパスワードを試す方法。


#### django-axes の機能

1. Django を使ったサイトへのログイン試行を記録し、設定された試行回数を超えると、攻撃者がサイトへのさらなるログイン試行を阻止。

2. ログイン試行を追跡し、データベースに無期限に保存することもできる。また、高速で DDoS 耐性のあるキャッシュの実装を使用することもできる。

3. IP アドレス、ユーザー名、ユーザーエージェント、またはそれらの組み合わせでログイン試行を監視するように設定できる。

4. クールオフ期間、IP アドレスのホワイトリストとブラックリスト、 ユーザアカウントのホワイトリスト、および Django のアクセス管理のためのその他の 機能をサポートしてくれる。


#### 実装方法

```
pip install django-axes
```

"Axes requires a supported Django version and runs on Python versions 3.6 and above." ということですから、サポートされている Django と 3.6以降のpython が必要。

[1. Requirements](https://django-axes.readthedocs.io/en/latest/1_requirements.html)

settings.py の INSTALLED_APPS  に 'axes'を追記。
```
INSTALLED_APPS = [
    #Axes app can be in any position in the INSTALLED_APPS list.
    'axes',

    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

]    
```
'axes'は、どこに記載してもいいようです。
***
2022/6/21 追記

INSTALLED_APPSには、記載順があるそうです。上記を一部変更しました。

[【Django】INSTALLED_APPS の適切な書き方](https://progress-advance.com/programming/django_installedapps/)


***

次に、settings.py の AUTHENTICATION_BACKENDS の一番上に「axes.backends.AxesBackend」

```
AUTHENTICATION_BACKENDS = [
    # AxesBackend should be the first backend in the AUTHENTICATION_BACKENDS list.
    'axes.backends.AxesBackend',

    # Django ModelBackend is the default authentication backend.
    'django.contrib.auth.backends.ModelBackend',
]
```

MIDDLEWARE の一番下に「axes.middleware.AxesMiddleware」を記載します。
```
MIDDLEWARE = [
    (中略) 

    # AxesMiddleware should be the last middleware in the MIDDLEWARE list.
    # It only formats user lockout messages and renders Axes lockout responses
    # on failed user authentication attempts from login views.
    # If you do not want Axes to override the authentication response
    # you can skip installing the middleware and use your own views.

    'axes.middleware.AxesMiddleware',
]
```

その後、「python manage.py check」で確認し、「python manage.py migrate」を実行します。

[2. Installation](https://django-axes.readthedocs.io/en/latest/2_installation.html)

---

AXES_FAILURE_LIMIT：ロックされるまでのログイン回数（整数）デフォルト（3）

AXES_LOCK_OUT_AT_FAILURE：この値がTrueの時、AXES_FAILURE_LIMIT の件数を超えるとロックが有効になる。デフォルト True

AXES_COOLOFF_TIME：自動でロックが解除されるまでの時間(単位は時間)

AXES_ONLY_USER_FAILURES：Trueにしたら、ロック対象をIPアドレスではなくユーザ名で判断する。デフォルト False

AXES_USE_USER_AGENT：Trueの場合、IPアドレスとユーザーエージェントに基づきロックアウトとログを記録する。これは、同じIPからの異なるユーザーからの要求が、別に扱われる。(AXES_ONLY_USER_FAILURES設定時は無効化)
デフォルト False


AXES_LOCKOUT_TEMPLATE：ロック画面のテンプレートを指定する。

AXES_LOCKOUT_URL：ロック画面のURLを指定する。(AXES_LOCKOUT_TEMPLATEを設定したらそちらを優先)

AXES_VERBOSE：Trueにしたら、Axesの詳細ログを見ることができる。

AXES_IP_BLACKLIST：ブラックリストにするIPアドレスを指定。

AXES_IP_WHITELIST：ホワイトリストにするIPアドレスを指定。

AXES_RESET_ON_SUCCESS：Trueのとき、ログイン成功で失敗回数リセット。

[4. Configuration](https://django-axes.readthedocs.io/en/latest/4_configuration.html)


Axes と Django REST Framework, Django Allauth, Django Simple Captcha, Django OAuth Toolkit, Django Reversion を併用するには、幾つかの設定が必要なようですが、別の機会に記載したいと思います。詳細は、「6.Integration」 をご確認ください。


[6. Integration](https://django-axes.readthedocs.io/en/latest/6_integration.html)


その他、参考にしたサイト。

[Djangoでログイン試行回数制限を付けられる「django-axes」の使い方](https://create-it-myself.com/know-how/how-to-use-django-axes/)

[Djangoのユーザ認証機能にログイン試行回数制限を追加する](https://create-it-myself.com/know-how/add-login-attempts-limit-to-django-user-authentication/)

[アカウントロック機能を追加するライブラリdjango-axesの紹介](https://medium.com/creditengine-tech/django-axes%E3%81%A7%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%83%AD%E3%83%83%E3%82%AF%E6%A9%9F%E8%83%BD%E4%BB%98%E3%81%8Ddjango%E3%82%A2%E3%83%97%E3%83%AA%E3%82%92%E9%96%8B%E7%99%BA%E3%81%99%E3%82%8B-e5414cc674e0)
