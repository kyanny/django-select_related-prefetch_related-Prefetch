# Prepare test data

```
python manage.py migrate
python manage.py shell
```

```
from polls.models import Choice, Question
from django.utils import timezone
import random

with open('/usr/share/dict/words') as f:
    words = [word.strip() for word in f.readlines()]

for i in range(10):
    question_text = ' '.join(random.sample(words, 5)).capitalize() + '?'
    q = Question(question_text=question_text, pub_date=timezone.now())
    q.save()
    for j in range(4):
        choice_text = ' '.join(random.sample(words, 2))
        q.choice_set.create(choice_text='Not much', votes=i)

```

# select_related

Without `select_related` (N+1 query)

```
for c in Choice.objects.all():
    c.question
```

<details>

```
(0.001) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice"; args=()
(0.001) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 1 LIMIT 21; args=(1,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 1 LIMIT 21; args=(1,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 1 LIMIT 21; args=(1,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 1 LIMIT 21; args=(1,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 2 LIMIT 21; args=(2,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 2 LIMIT 21; args=(2,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 2 LIMIT 21; args=(2,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 2 LIMIT 21; args=(2,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 3 LIMIT 21; args=(3,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 3 LIMIT 21; args=(3,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 3 LIMIT 21; args=(3,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 3 LIMIT 21; args=(3,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 4 LIMIT 21; args=(4,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 4 LIMIT 21; args=(4,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 4 LIMIT 21; args=(4,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 4 LIMIT 21; args=(4,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 5 LIMIT 21; args=(5,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 5 LIMIT 21; args=(5,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 5 LIMIT 21; args=(5,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 5 LIMIT 21; args=(5,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 6 LIMIT 21; args=(6,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 6 LIMIT 21; args=(6,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 6 LIMIT 21; args=(6,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 6 LIMIT 21; args=(6,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 7 LIMIT 21; args=(7,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 7 LIMIT 21; args=(7,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 7 LIMIT 21; args=(7,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 7 LIMIT 21; args=(7,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 8 LIMIT 21; args=(8,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 8 LIMIT 21; args=(8,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 8 LIMIT 21; args=(8,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 8 LIMIT 21; args=(8,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 9 LIMIT 21; args=(9,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 9 LIMIT 21; args=(9,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 9 LIMIT 21; args=(9,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 9 LIMIT 21; args=(9,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 10 LIMIT 21; args=(10,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 10 LIMIT 21; args=(10,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 10 LIMIT 21; args=(10,)
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question" WHERE "polls_question"."id" = 10 LIMIT 21; args=(10,)
```

</details>

With `select_related`

```
for c in Choice.objects.select_related('question').all():
    c.question
```

<details>

```
(0.002) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes", "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_choice" INNER JOIN "polls_question" ON ("polls_choice"."question_id" = "polls_question"."id"); args=()
```

</details>

# prefetch_related

Without `prefetch_related` (N+1 query)

```
for q in Question.objects.all():
    for c in q.choice_set.all():
        c
```

<details>

```
(0.004) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question"; args=()
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 1; args=(1,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 2; args=(2,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 3; args=(3,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 4; args=(4,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 5; args=(5,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 6; args=(6,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 7; args=(7,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 8; args=(8,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 9; args=(9,)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" = 10; args=(10,)
```

</details>

With `prefetch_related`

```
for q in Question.objects.prefetch_related('choice_set').all():
    for c in q.choice_set.all():
        c
```

<details>

```
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question"; args=()
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" IN (1, 2, 3, 4, 5, 6, 7, 8, 9, 10); args=(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

</details>

# Prefetch

Without `Prefetch`

```
for q in Question.objects.prefetch_related('choice_set').all():
    for c in q.choice_set.filter(votes=0):
        c
```

<details>

```
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question"; args=()
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE "polls_choice"."question_id" IN (1, 2, 3, 4, 5, 6, 7, 8, 9, 10); args=(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
(0.001) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 1 AND "polls_choice"."votes" = 0); args=(1, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 2 AND "polls_choice"."votes" = 0); args=(2, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 3 AND "polls_choice"."votes" = 0); args=(3, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 4 AND "polls_choice"."votes" = 0); args=(4, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 5 AND "polls_choice"."votes" = 0); args=(5, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 6 AND "polls_choice"."votes" = 0); args=(6, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 7 AND "polls_choice"."votes" = 0); args=(7, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 8 AND "polls_choice"."votes" = 0); args=(8, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 9 AND "polls_choice"."votes" = 0); args=(9, 0)
(0.000) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."question_id" = 10 AND "polls_choice"."votes" = 0); args=(10, 0)
```

</details>

With `Prefetch`

```
from django.db import models
from django.db.models import Prefetch

for q in Question.objects.prefetch_related(Prefetch('choice_set', queryset=Choice.objects.filter(votes=0))).all():
    for c in q.choice_set.all():
        c
```

<details>

```
(0.000) SELECT "polls_question"."id", "polls_question"."question_text", "polls_question"."pub_date" FROM "polls_question"; args=()
(0.001) SELECT "polls_choice"."id", "polls_choice"."question_id", "polls_choice"."choice_text", "polls_choice"."votes" FROM "polls_choice" WHERE ("polls_choice"."votes" = 0 AND "polls_choice"."question_id" IN (1, 2, 3, 4, 5, 6, 7, 8, 9, 10)); args=(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

</details>
