---
layout: post
title: "Routers in django and when to use them"
date: 2025-01-23
categories: django
---


First of all what are routers, but before we go into that, there are two ways to register a ViewSet method

1. Manual URL mapping
2. Using the default router (conventional way)
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
