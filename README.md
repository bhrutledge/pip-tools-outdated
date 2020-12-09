# Identifying outdated dependencies with pip-tools

Given the high-level dependencies in [requirements.in](./requirements.in), and the current frozen packages in [requirements.txt](./requirements.txt) (compiled by [pip-tools](https://github.com/jazzband/pip-tools)), I would like to know which of my high-level dependencies are outdated.

Unfortunately, `pip list --outdated` shows _all_ outdated packages in the active Python environment:

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

There are open issues to filter by a requirements file:

- [Allow `pip list --outdated` to take a requirements file · Issue #3314 · pypa/pip](https://github.com/pypa/pip/issues/3314)
- [Merge feature from pip-outdated - list outdated packages from requirements file · Issue #7895 · pypa/pip](https://github.com/pypa/pip/issues/7895)
- [Feature: A command like 'pip list -o' that only shows outdated packages in requirements.in · Issue #1167 · jazzband/pip-tools](https://github.com/jazzband/pip-tools/issues/1167)

For now, we can take advantage of the `# via` comments added by pip-tools to identify the dependencies that are declared in `requirements.in`:

```
$ grep 'via -r' requirements.txt | tee requirements.in.txt
celery==4.2.2             # via -r requirements.in
django-cms==3.7.2         # via -r requirements.in
django==2.2.15            # via -r requirements.in, django-classy-tags, django-cms, django-formtools, django-sekizai, django-treebeard, djangorestframework
djangorestframework==3.11.0  # via -r requirements.in
requests==2.21.0          # via -r requirements.in
```

Then, we can use that as input to one of several tools that operate on requirements files.

## [alanhamlett/pip-update-requirements](https://github.com/alanhamlett/pip-update-requirements)

```
$ pur -r requirements.in.txt
Updated celery: 4.2.2 -> 5.0.4
Updated django-cms: 3.7.2 -> 3.8.0
Updated django: 2.2.15 -> 3.1.4
Updated djangorestframework: 3.11.0 -> 3.12.2
Updated requests: 2.21.0 -> 2.25.0
```

This updates `requirements.in.txt` with the new versions, but does not install the new versions.

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

If we didn't quit, this would install the new versions and update `requirements.in.txt`.

## [eight04/pip-outdated](https://github.com/eight04/pip-outdated)

```
$ pip-outdated requirements.in.txt 
Red = unavailable/outdated/out of version specifier
Green = updatable
+---------------------+-----------+--------+--------+
| Name                | Installed | Wanted | Latest |
+---------------------+-----------+--------+--------+
| celery              | 4.2.2     | 4.2.2  | 5.0.4  |
| django-cms          | 3.7.2     | 3.7.2  | 3.8.0  |
| django              | 2.2.15    | 2.2.15 | 3.1.4  |
| djangorestframework | 3.11.0    | 3.11.0 | 3.12.2 |
| requests            | 2.21.0    | 2.21.0 | 2.25.0 |
+---------------------+-----------+--------+--------+
```

This doesn't modify any files or install packages. Requires Python 3.7+.
