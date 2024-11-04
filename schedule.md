---
layout: schedule
title: Course Schedule 
description: Being less concrete further out, the course scheduling is tentative and subject to changes.
nav_order: 2
weeks:
  # Each key in this dictionary is a week, and then eaach week has a key in [Mon, Tue, Wed, Thu, Fri].
  # Each day has keys `date` and `content`. The date is shown on the schedule, and `content` is a key into the yml file in \_data/modules.yml. `content` may be an array.
  # Each day can also have a `note` field, which is shown in italics on the calendar.
  # This schedule data is unioned with the deadlines in \_data/config.yml
  '1':
    Mon:
      date: 2024/08/26
      content: fMonday
    Wed:
      date: 2024/08/28
      content: 1a
  '2':
    Mon:
      date: 2024/09/02
      content: 2a
      note: "[Lab 0](/cs4740-fall24/labs/lab0) out"
    Wed:
      date: 2024/09/04
      content: 3a
  '3':
    Mon:
      date: 2024/09/09
      content: 4a
    Wed:
      date: 2024/09/11
      content: 5a
      note: "[Lab 1](/cs4740-fall24/labs/lab1) out"
  '4':
    Mon:
      date: 2024/09/16
      content: 6a
    Wed:
      date: 2024/09/18
      content: 7a
  '5':
    Mon:
      date: 2024/09/23
      content: 8a
    Wed:
      date: 2024/09/25
      content: 8b
  '6':
    Mon:
      date: 2024/09/30
      content: 9a
      note: "[Lab 2](/cs4740-fall24/labs/lab2) out"
    Wed:
      date: 2024/10/02
      content: 9b
  '7':
    Mon:
      date: 2024/10/07
      content: 10a
    Wed:
      date: 2024/10/09
      content: 11a
  '8':
    Mon:
      date: 2024/10/14
      content: fallReadingDay
    Wed:
      date: 2024/10/16
      content: midtermReview
  '9':
    Mon:
      date: 2024/10/21
      content: midterm
      note: "[Lab 3a](/cs4740-fall24/labs/lab3#part-3a) out"
    Wed:
      date: 2024/10/23
      content: 11b
  '10':
    Mon:
      date: 2024/10/28
      content: 12a
    Wed:
      date: 2024/10/30
      content: 12b
  '11':
    Mon:
      date: 2024/11/04
      content: 13a
      note: "[Lab 3b](/cs4740-fall24/labs/lab3#part-3b) out"
    Wed:
      date: 2024/11/06
      content: 14a
  '12':
    Mon:
      date: 2024/11/11
      content: 14b
    Wed:
      date: 2024/11/13
      content: 15a
  '13':
    Mon:
      date: 2024/11/18
      content: 15b
    Wed:
      date: 2024/11/20
      content: 16a
  '14':
    Mon:
      date: 2024/11/25
      content: 17a
    Wed:
      date: 2024/11/27
      content: thanksGiving
  '15':
    Mon:
      date: 2024/12/02
      content: finalReview
    Wed:
      date: 2024/12/04
      content: readingDay
  '16':
    Mon:
      date: 2024/12/09
      content: readingDay
    Wed:
      date: 2024/12/11
      content: readingDay
    Fri:
      date: 2024/12/13
      content: finalExam
---
