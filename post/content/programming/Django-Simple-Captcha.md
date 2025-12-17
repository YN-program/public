---
title: "Django Simple Captcha"
description: ""
date: "2022-04-13T17:01:04+09:00"
thumbnail: "img/captcha3.png"
categories:
  - "programming"
tags:
  - "django"
menu:
main:
  name: プログラミング
  weight: 2
---
ボットによる不正アカウント作成の防止、スパム対策のための「CAPTCHA」。

Google reCAPTCHA は、クレジットカードの登録が必要なのと、100万件を超えると課金されるので、Google reCAPTCHA ではなく、django-simple-captcha を試しにインストールしてみました。

<!--more-->

google reCAPTCHA を使用する場合は、こちらから ([Django reCAPTCHA](https://github.com/torchbox/django-recaptcha#installation))。

#### Django Simple Captcha

[Django Simple Captcha](https://django-simple-captcha.readthedocs.io/en/latest/?msclkid=ca0f265bbaff11ec8d5dcbe24d6aab37)


#### インストール

```
pip install django-simple-captcha
```

次に、settings.py の INSATALED_APPS に 「'captcha',」 と記載します。

```
INSTALLED_APPS = [
 (中略)
  'axes',
  'captcha',
]
```
前回記事でも書きましたが、「'axes',」の「,」を忘れると、以下のようなエラーが出ます。
```
ModuleNotFoundError: No module named 'axescaptcha'
```
で、「python manage.py check」で問題なければ、「python manage.py migrate」を実行します。

次に、urls.py に
```
urlpatterns += [
    path('captcha/', include('captcha.urls')),
]
```
と記載します。

ちなみに、Pillow も必要なので、インストールしておきます。

[instration](https://django-simple-captcha.readthedocs.io/en/latest/usage.html#installation)

[Pillow](https://pypi.org/project/Pillow/)

#### django-axes との併用

django-axes と django simple captcha の併用には、幾つか設定が必要ですので、以下のドキュメントからコピー。

[Integration with Django Simple Captcha](https://django-axes.readthedocs.io/en/latest/6_integration.html#integration-with-django-simple-captcha)

>settings.py:
>```
>AXES_LOCKOUT_URL = '/locked'
>```
>example/urls.py:
>```
>url(r'^locked/$', locked_out, name='locked_out'),
>```
>example/forms.py:
>```
>class AxesCaptchaForm(forms.Form):
>    captcha = CaptchaField()
>```
>example/views.py:
>```
>from axes.utils import reset_request
>from django.http.response import HttpResponseRedirect
>from django.shortcuts import render
>from django.urls import reverse_lazy
>
>from .forms import AxesCaptchaForm
>
>
>def locked_out(request):
>    if request.POST:
>        form = AxesCaptchaForm(request.POST)
>        if form.is_valid():
>            reset_request(request)
>            return HttpResponseRedirect(reverse_lazy('auth_login'))
>    else:
>        form = AxesCaptchaForm()
>
>    return render(request, 'accounts/captcha.html', {'form': form})
>```
>example/templates/example/captcha.html:
>```
> <form action="" method="post">
>    {% csrf_token %}
>
>    {{ form.captcha.errors }}
>    {{ form.captcha }}
>
>    <div class="form-actions">
>        <input type="submit" value="Submit" />
>    </div>
> </form>
>```

試しに実装してみたものの、使用した印象としてはユーザビリティをかなり損なうので、もっと使いやすいものがあればそちらのほうが良いかもしれません。
今後の課題となりそうです。

[CAPTCHA、音声、スライダーetc…フォームのスパム対策方法まとめ](https://f-tra.com/ja/blog/column/4448)