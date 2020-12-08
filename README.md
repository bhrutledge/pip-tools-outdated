# Outdated packages with pip-tools

[Feature: A command like 'pip list -o' that only shows outdated packages in requirements.in · Issue #1167 · jazzband/pip-tools](https://github.com/jazzband/pip-tools/issues/1167)

```
$ pip list --outdated
Package             Version Latest  Type
------------------- ------- ------- -----
amqp                2.6.1   5.0.2   wheel
billiard            3.5.0.5 3.6.3.0 wheel
celery              4.2.2   5.0.4   wheel
Django              2.2.15  3.1.4   wheel
django-cms          3.7.2   3.8.0   wheel
djangorestframework 3.11.0  3.12.2  wheel
idna                2.8     2.10    wheel
kombu               4.3.0   5.0.2   wheel
requests            2.21.0  2.25.0  wheel
urllib3             1.24.3  1.26.2  wheel
vine                1.3.0   5.0.0   wheel
```

```
$ grep 'via -r' requirements.txt | tee requirements.in.txt
celery==4.2.2             # via -r requirements.in
django-cms==3.7.2         # via -r requirements.in
django==2.2.15            # via -r requirements.in, django-classy-tags, django-cms, django-formtools, django-sekizai, django-treebeard, djangorestframework
djangorestframework==3.11.0  # via -r requirements.in
requests==2.21.0          # via -r requirements.in
```

## [alanhamlett/pip-update-requirements](https://github.com/alanhamlett/pip-update-requirements):

```
$ pur -r requirements.in.txt
Updated celery: 4.2.2 -> 5.0.4
Updated django-cms: 3.7.2 -> 3.8.0
Updated django: 2.2.15 -> 3.1.4
Updated djangorestframework: 3.11.0 -> 3.12.2
Updated requests: 2.21.0 -> 2.25.0
```

Also updates `requirements.in.txt` with the new versions.

## [simion/pip-upgrader](https://github.com/simion/pip-upgrader)

## [bartTC/pip-check](https://github.com/bartTC/pip-check/)
