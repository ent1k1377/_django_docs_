# Django Docs 4.1

Полная документация [django](https://docs.djangoproject.com/en/4.1/).

## Дополнительные ссылки
- [GitBooks по созданию Django-проекта](https://pocoz.gitbooks.io/django-v-primerah/content/) - много русскоязычной информации

## Static Files

<https://docs.djangoproject.com/en/4.1/ref/contrib/staticfiles/>

Настройки осуществляются с помощью встроенного приложения `django.contrib.staticfiles`, именно он собирает статические файлы в одном месте.

___

### STATIC_ROOT

Дефолт: `None`

Устанавливает абсолютный путь к папке в которой будут собираться все статические файлы проекта. То есть в этой папке
будут храниться вся статика из всех приложений.<br>
Бесполезен при локальной разработке, нужен только при развертывании в Nginx, Apache и т.д.

```
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

### STATIC_URL

Дефолт: `None`

URL для использования при обращении к статическим файлам, расположенным в STATIC_ROOT.

```
STATIC_URL = 'static/'
```

### STATICFILES_DIRS

Дефолт: `[]`

Список папок/приложений в которых будут изъяты дополнительные статические файлы.

```
STATICFILES_DIRS = (
    BASE_DIR / 'static',
)
```

### STATICFILES_STORAGE

Дефолт: `'django.contrib.staticfiles.storage.StaticFilesStorage'`<br>
Забей на это.

### STATICFILES_FINDERS

Дефолт:

    [
        'django.contrib.staticfiles.finders.FileSystemFinder',
        'django.contrib.staticfiles.finders.AppDirectoriesFinder',
    ]

Список поисковых серверов, которые умеют находить статические файлы в разных местах.
___

## MEDIA

<https://docs.djangoproject.com/en/4.1/topics/files/><br>
<https://docs.djangoproject.com/en/4.1/howto/static-files/>

### MEDIA_ROOT

Дефолт: `''`

Устанавливает абсолютный путь к папке в которой будут собираться все медиа файлы.

Пример:
`MEDIA_ROOT = os.path.join(BASE_DIR, 'media')`

### MEDIA_URL

Дефолт: `''`

URL-адрес, который обрабатывает медиафайлы из MEDIA_ROOT и используется для управления сохраненными файлами.

Пример:
`MEDIA_URL = '/media/'`

### Дополнительно

Во время отладки/разработки нужно включить MEDIA_URL таким способом, чтобы можно было загружать файлы с сайта в хранилище/диск.

```
# файл app/app/urls.py
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```
---

## Templates

<https://metanit.com/python/django/2.5.php>

Дефолт:

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]

Отвечает за конфигурацию шаблонов в проекте.

### BACKEND

Движок шаблонов, по умолчанию [DjangoTemplates](https://docs.djangoproject.com/en/4.1/topics/templates/), так же можно Jinja2.

### DIRS

Определяет список каталогов, где движок будет искать файлы шаблонов. 
По умолчанию движок ищет в папке templates каждого приложения. 
Объявивив `'DIRS': [os.path.join(BASE_DIR, 'templates')]`, движок будет искать шаблоны на уровне проекта, а только потом в приложениях.<br>
Best Practices [Template Structure](https://learndjango.com/tutorials/template-structure)

### APP_DIRS

Указывает, будет ли движок работать.

### OPTIONS

Указывает дополнительный список параметров. 
В частности, указывает, какие обработчики (`context_processors`) будут использоваться при обработке шаблонов.
`context_processors`добавляет в любой шаблон собственные переменные, с которыми удобно взаимодействовать.<br>
Например `django.contrib.auth.context_processors.auth` добавляет переменную `user`, в которой лежит либо анонимный пользователь либо найденный в систему.