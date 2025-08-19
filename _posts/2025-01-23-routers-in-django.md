---
layout: post
title: "Routers in django and when to use them"
date: 2025-01-23
categories: [django, python]
---

First of all what are routers, but before we go into that, there are two ways to register a ViewSet method

### 1. Manual URL mapping
{% highlight python linenos %}
path('votes/', VoteView.as_view({'get': 'get_all_votes'}))
{% endhighlight %}
Here, the method can be anything because you're explicitly telling  Django which method to call
### 2. Using the default router (conventional way)
{% highlight python linenos %}
from rest_framework.routers import DefaultRouter
router = DefaultRouter()
router.register()

from rest_framework import routers

router = routers.SimpleRouter()
router.register(r'users', UserViewSet)
router.register(r'accounts', AccountViewSet)
urlpatterns = router.urls
{% endhighlight %}

{% highlight python linenos %}
from rest_framework.routers import DefaultRouter
router = DefaultRouter()
router.register('votes', VoteView)

from rest_framework import routers
urlpatterns = router.urls
{% endhighlight %}

in this case, the method names matter because the router automatically maps:

- `list()` -> GET /votes/
- `retrieve()` -> GET /votes/{id}/

### Pros of using router
- Follows REST framework conventions
- Makes code more maintainable

### Pros of using manual approach
- More descriptive method name

### 1. Manual URL mapping
{% highlight python linenos %}
path('votes/', VoteView.as_view({'get': 'get_all_votes'}))
{% endhighlight %}

### 2. Using the default router (conventional way)
{% highlight python linenos %}
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register('votes', VoteView)

# This automatically creates:
# GET /votes/ -> list()
# POST /votes/ -> create()
# GET /votes/{id}/ -> retrieve()
{% endhighlight %}

### Summary:
When you use the router approach, the method names matter because the router looks for specific method names like list, create, etc).