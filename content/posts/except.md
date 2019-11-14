---
title: "Django Core Exceptions"
date: "2019-11-12T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/except/"
category: "Django"
tags:
description: "#DoesNotExist vs ObjectsDoesNotExist"
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### 1. ObjectDoesNotExist and DoesNotExist
+ exception **DoesNotExist** --> 특정 모델에만 이용 가능  
<br>The DoesNotExist exception is raised when an object is not found for the given parameters of a query. Django provides a DoesNotExist exception as an attribute of each model class to identify the class of object that could not be found and to allow you to catch a particular model class with try/except.

+ exception **ObjectDoesNotExist**--> 모든 모델 객체에서 이용 가능  
<br>(from django.core.exceptions import ObjectDoesNotExist 필요)  
The base class for DoesNotExist exceptions; a try/except for ObjectDoesNotExist will catch DoesNotExist exceptions for all models.

### 하나씩 정리해야지...
+ MultipleObjectsReturned
+ SuspiciousOperation
+ PermissionDenied
