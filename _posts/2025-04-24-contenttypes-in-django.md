---
layout: post
title: "Understanding Django ContentTypes: When and How to Use Them"
date: 2025-04-22
categories: [django, python]
---

Do you know Django keeps track of your models, what that means is that [bla bla] in this article I break down how to use and understand Django's contenttype


Django includes a contenttypes framework located at `django.contrib.contenttypes`. This application can track all models installed in your project and provides a generic interface to interact with your models

The `django.contrib.contenttypes` application is included in the `INSTALLED_APPS` setting by default
when you create a new project using the startproject command. It is used by other contrib packages,
such as the authentication framework and the administration application.

The contenttypes application contains a ContentType model. Instances of this model represent the
actual models of your application, and new instances of ContentType are automatically created when
new models are installed in your project. The ContentType model has the following fields:

- app_label: This indicates the name of the application that the model belongs to. This is au-
tomatically taken from the app_label attribute of the model Meta options. For example, your
Image model belongs to the images application.
- model: The name of the model class.
- name: This indicates the human-readable name of the model. This is automatically taken from
the verbose_name attribute of the model Meta options.