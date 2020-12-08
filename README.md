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

## [alanhamlett/pip-update-requirements](https://github.com/alanhamlett/pip-update-requirements)

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

```
$ echo q | pip-upgrade requirements.in.txt 
Found valid requirements file(s): 
requirements.in.txt
1/5: celery ... upgrade available: 4.2.2 ==> 5.0.4 (uploaded on 2020-12-08 12:51:27)
2/5: django-cms ... upgrade available: 3.7.2 ==> 3.8.0 (uploaded on 2020-10-28 12:25:57)
3/5: django ... upgrade available: 2.2.15 ==> 3.1.4 (uploaded on 2020-12-01 06:03:32)
4/5: djangorestframework ... upgrade available: 3.11.0 ==> 3.12.2 (uploaded on 2020-11-05 22:04:52)
5/5: requests ... upgrade available: 2.21.0 ==> 2.25.0 (uploaded on 2020-11-11 20:05:15)

Available upgrades:
+-----+-----------------------+-----------------+----------------+---------------------+
| No. | Package               | Current version | Latest version | Release date        |
+-----+-----------------------+-----------------+----------------+---------------------+
|  1  |  celery               | 4.2.2           | 5.0.4          | 2020-12-08 12:51:27 |
|  2  |  django-cms           | 3.7.2           | 3.8.0          | 2020-10-28 12:25:57 |
|  3  |  django               | 2.2.15          | 3.1.4          | 2020-12-01 06:03:32 |
|  4  |  djangorestframework  | 3.11.0          | 3.12.2         | 2020-11-05 22:04:52 |
|  5  |  requests             | 2.21.0          | 2.25.0         | 2020-11-11 20:05:15 |
+-----+-----------------------+-----------------+----------------+---------------------+

Please choose which packages should be upgraded. Choices: "all", "q" (quit), "x" (exit) or "1 2 3"
Choice: Quit.

Upgrade interrupted.
```

## [bartTC/pip-check](https://github.com/bartTC/pip-check/)
