---
title: "【Django】AND 検索 OR 検索"
description: ""
date: "2022-05-02T13:06:02+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "django"
  - "python"
menu:
main:
  name: プログラミング
  weight: 2
---

検索機能に and 検索と or 検索の両方を付けて、ミュートした単語とリクエストユーザーをブロックしているユーザーのコンテンツを除外する機能を付けたくて、試行錯誤してみました。ついでに、複数のモデルでやってみました。

<!--more-->

`sample/search.html`

```
<div>
    <form action="{% url 'sample:search' %}" method="GET">
        <input type="text" name="search"  placeholder="キーワード検索" >
        <button type="submit" >検索</button>
    </form>
</div>
```

モデル例。

`sample/models.py`

```
class Sample1(models.Model):

    class Meta:

        db_table = "sample1"

    id           = models.UUIDField(default=uuid.uuid4, primary_key=True, editable=False )
    title        = models.CharField(verbose_name="タイトル", max_length=50)
    description  = models.CharField(verbose_name="説明文", max_length=500)
    user         = models.ForeignKey(settings.AUTH_USER_MODEL, verbose_name="ユーザー", on_delete=models.CASCADE)


class Sample2(models.Model):

    class Meta:

        db_table = "sample2"

    id           = models.UUIDField(default=uuid.uuid4, primary_key=True, editable=False )
    title        = models.CharField(verbose_name="タイトル", max_length=50)
    description  = models.CharField(verbose_name="説明文", max_length=500)
    user         = models.ForeignKey(settings.AUTH_USER_MODEL, verbose_name="ユーザー", on_delete=models.CASCADE)

class Mute(models.Model)    :

    class Meta:
        db_table = "mute"
        
    id           = models.UUIDField(default=uuid.uuid4, primary_key=True, editable=False )
    mute         = models.CharField(verbose_name="ミュートする単語", max_length=50)
    user         = models.ForeignKey(settings.AUTH_USER_MODEL, verbose_name="ユーザー", on_delete=models.CASCADE) 
       
```

ユーザーモデル例。

`users/models.py`

```
class CustomUser(AbstractBaseUser, PermissionsMixin):
    id        =
    username  = 
    ~~~~中略~~~~
    blocked   = models.ManyToManyField("self",through="BlockUser" ,through_fields=('to_user', 'from_user'), verbose_name="ブロック",blank=True)


class BlockUser(models.Model):

    class Meta:
        db_table    = "blockuser"

    id          = models.UUIDField( default=uuid.uuid4, primary_key=True, editable=False )
    dt          = models.DateTimeField(verbose_name="ブロックした日時",default=timezone.now)
    from_user   = models.ForeignKey(CustomUser,verbose_name="ブロック元のユーザー",on_delete=models.CASCADE,related_name="block_from_user")
    to_user     = models.ForeignKey(CustomUser,verbose_name="ブロック対象のユーザー",on_delete=models.CASCADE,related_name="block_to_user")


```
`settings.py`

検索数の上限を記載しておく。

```
DEFAULT_AMOUNT = 100
```

`views.py`

```
from django.db.models import Q

class SearchView(views.APIView):

    def get(self, request, *args, **kwargs):

        context = {}
        if "search" in request.GET:
            search = request.GET["search"]

            if search == "" or search.isspace():
                return render(request, "sample/search.html", context)

        search_split = search.replace("　", " ").split(" ")
        search_list = [s for s in search_split if s != ""]

        query_and = Q()
        query_or  = Q()
        query_ex  = Q()

        for s in search_list:
            query_and &= Q( Q(title__icontains=s) | Q(description__icontains=s) | Q(user__handle_name__icontains=s) )
            query_or  |= Q( Q(title__icontains=s) | Q(description__icontains=s) | Q(user__handle_name__icontains=s) )

        if request.user.is_authenticated:
             # リクエストユーザーをブロックしているユーザーのid
            block_list = list(BlockUser.objects.filter(to_user=request.user.id).values_list("from_user_id", flat=True)) 

            #ミュートした単語          
            Mute_list = list( Mute.objects.filter(user=request.user.id).values_list('mute', flat=True) )
            if Mute_list:
                for m in Mute_list:
                    query_ex |= Q( Q(title__icontains=m) | Q(description__icontains=m) | Q(user__handle_name__icontains=m) )
              
            #sample_1 は、or 検索のみで、ミュートした単語は除外。
            context["sample_1"] = Sample1.objects.filter(query_or & ~query_ex).exclude(user__id__in=block_list).order_by("-dt")

            #sample_2は、and 検索の後、or 検索を追加。どちらもミュートした単語は除外。ログイン状態では、リクエストユーザーをブロックしている人のものは検索対象外に。

            sample_2_and = list( Sample2.objects.filter(query_and & ~query_ex).exclude( user__id__in=block_list) )

            if len(sample_2_and) < settings.DEFAULT_AMOUNT:
                sample_2_or = list( Sample2.objects.filter(query_or & ~query_ex).exclude(user__id__in=block_list) )
                s        = sample_2_and + sample_2_or
                sample_2 = sorted( list(set(s)), key=s.index)

        else:
            context["sample_1"] = Sample1.objects.filter(query_and | query_or).order_by("-dt")

            sample_2_and = liset( Sample2.objects.filter(query_and) )

            if len(sample_2_and) < settings.DEFAULT_AMOUNT:
                sample_2_or = list( Sample2.objects.filter(query_or) )
                s        = sample_2_and + sample_2_or
                sample_2 = sorted( list(set(s)), key=s.index)

        context["sample_2"] = sample_2


        return render(request, "sample/search.html", context)

search = SearchView.as_view()

```

これで、2つのモデルで検索ができる。

`sample_1`は or 検索のみになっているが、必要に応じて and 検索も追加できる。

`sample_2`は、and 検索の結果の後に、or 検索の結果が来るようになっている。

また、ミュートした単語とリクエストユーザーをブロックしているユーザーのコンテンツは除外できる。

and 検索に or 検索の結果が追加されて、リストの要素数が際限なく増える可能性があるときは、インデックスを使って必要な分だけ取り出すようにする。

参考にしたサイト。

[【Django】スペース区切りでOR・AND検索を改定する](https://noauto-nolife.com/post/django-or-and-search-revision/)

[【Django】カスタムユーザーモデルでユーザーブロック機能を実装させる【ManyToManyFieldでユーザーモデル自身を指定】](https://noauto-nolife.com/post/django-m2m-usermodel/)

[Django入門｜OR 条件でクエリセットを取得する方法](https://dot-blog.jp/news/django-queryset-or-filter/)

[【python】listをuniqueにする方法【重複要素の削除】](https://lanchesters.site/python-list-uniq/)

[［解決！Python］リスト（配列）の要素にインデックスやスライスを使ってアクセスするには](https://atmarkit.itmedia.co.jp/ait/articles/2012/08/news013.html)