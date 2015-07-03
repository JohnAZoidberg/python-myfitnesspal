[![Build Status](https://travis-ci.org/coddingtonbear/python-myfitnesspal.png?branch=master)](https://travis-ci.org/coddingtonbear/python-myfitnesspal)  [![Join the chat at https://gitter.im/coddingtonbear/python-myfitnesspal](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/coddingtonbear/python-myfitnesspal?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

MyFitnessPal
============

Do you track your eating habits on [MyFitnessPal](https://www.myfitnesspal.com/)?  Have you ever wanted to analyze the information you're entering into MyFitnessPal programatically?  

Although MyFitnessPal [does have an API](https://www.myfitnesspal.com/api), it is private-access only; this creates an unnecessary barrier between you and your data that can be overcome using this library.

Having problems? [Issues live on github](https://github.com/coddingtonbear/python-myfitnesspal/issues).  Have questions?  [Ask your questions in our gitter room](https://github.com/coddingtonbear/python-myfitnesspal/pull/17).

Installation
------------

You can either install from pip:

    pip install myfitnesspal

*or* checkout and install the source from the [github repository](https://github.com/coddingtonbear/python-myfitnesspal):

    git clone https://github.com/coddingtonbear/python-myfitnesspal.git
    cd python-myfitnesspal
    python setup.py install


Diary Examples
--------------

To access a single day's information:

```python
import myfitnesspal

client = myfitnesspal.Client('my_username', 'my_password')

day = client.get_date(2013, 3, 2)
day
# >> <03/02/13 {'sodium': 3326, 'carbohydrates': 369, 'calories': 2001, 'fat': 22, 'sugar': 103, 'protein': 110}>
```

To see all meals you can use the Day object's `meals` property:

```python
day.meals
# >> [<Breakfast {}>,
#    <Lunch {'sodium': 712, 'carbohydrates': 106, 'calories': 485, 'fat': 3, 'sugar': 0, 'protein': 17}>,
#    <Dinner {'sodium': 2190, 'carbohydrates': 170, 'calories': 945, 'fat': 11, 'sugar': 17, 'protein': 53}>,
#    <Snacks {'sodium': 424, 'carbohydrates': 93, 'calories': 571, 'fat': 8, 'sugar': 86, 'protein': 40}>]
```

To access dinner, you can access it by its index in `day.meals`:

```python
dinner = day.meals[2]
dinner
# >> <Dinner {'sodium': 2190, 'carbohydrates': 170, 'calories': 945, 'fat': 11, 'sugar': 17, 'protein': 53}>
```

To get a list of things you ate for dinner, I can use the dinner Meal object's `entries` property:

```python
dinner.entries
# >> [<Montebello - Spaghetti noodles, 6 oz. {'sodium': 0, 'carbohydrates': 132, 'calories': 630, 'fat': 3, 'sugar': 3, 'protein': 21}>,
#     <Fresh Market - Arrabiatta Organic Pasta Sauce, 0.5 container (3 cups ea.) {'sodium': 1410, 'carbohydrates': 24, 'calories': 135, 'fat': 5, 'sugar': 12, 'protein': 6}>,
#     <Quorn - Meatless and Soy-Free Meatballs, 6 -4 pieces (68g) {'sodium': 780, 'carbohydrates': 14, 'calories': 180, 'fat': 3, 'sugar': 2, 'protein': 26}>]
```

To access one of the items, use the entries property as a list:

```python
spaghetti = dinner.entries[0]
spaghetti.name
# >> Montebello - Spaghetti noodles, 6 oz.
```

For a daily summary of your nutrition information, you can use a Day object's `totals` property:

```python
day.totals
# >> {'calories': 2001,
#     'carbohydrates': 369,
#     'fat': 22,
#     'protein': 110,
#     'sodium': 3326,
#     'sugar': 103}
```

Or, if you just want to see how many cups of water you've recorded, or the notes you've entered for a day:

```python
day.water
# >> 1
day.notes
# >> "This is the note I entered for this day"
```

For just one meal:

```python
dinner.totals
# >> {'calories': 945,
#     'carbohydrates': 170,
#     'fat': 11,
#     'protein': 53,
#     'sodium': 2190,
#     'sugar': 17}
```

For just one entry:

```python
spaghetti.totals
# >> {'calories': 630,
#     'carbohydrates': 132,
#     'fat': 3,
#     'protein': 21,
#     'sodium': 0,
#     'sugar': 3}
```

Measurement Examples
--------------------

To access measurements from the past 30 days:

```python
import myfitnesspal

client = myfitnesspal.Client('my_username', 'my_password')

weight = client.get_measurements('Weight')
weight
# >> OrderedDict([(datetime.date(2015, 5, 14), 171.0), (datetime.date(2015, 5, 13), 173.8), (datetime.date(2015, 5,12), 171.8),
#                 (datetime.date(2015, 5, 11), 171.6), (datetime.date(2015, 5, 10), 172.4), (datetime.date(2015, 5, 9), 170.2),
#                 (datetime.date(2015, 5, 8), 171.0),  (datetime.date(2015, 5, 7), 171.2),  (datetime.date(2015, 5, 6), 170.8),
#                 (datetime.date(2015, 5, 5), 171.8),  (datetime.date(2015, 5, 4), 174.2),  (datetime.date(2015, 5, 3), 172.2),
#                 (datetime.date(2015, 5, 2), 171.0),  (datetime.date(2015, 5, 1), 171.2),  (datetime.date(2015, 4, 30), 171.6),
#                 (datetime.date(2015, 4, 29), 172.4), (datetime.date(2015, 4, 28), 172.2), (datetime.date(2015, 4, 27), 173.2),
#                 (datetime.date(2015, 4, 26), 171.8), (datetime.date(2015, 4, 25), 170.8), (datetime.date(2015, 4, 24), 171.2),
#                 (datetime.date(2015, 4, 23), 171.6), (datetime.date(2015, 4, 22), 173.2), (datetime.date(2015, 4, 21), 174.2),
#                 (datetime.date(2015, 4, 20), 173.6), (datetime.date(2015, 4, 19), 171.8), (datetime.date(2015, 4, 18), 170.4),
#                 (datetime.date(2015, 4, 17), 169.8), (datetime.date(2015, 4, 16), 170.4), (datetime.date(2015, 4, 15), 170.8),
#                 (datetime.date(2015, 4, 14), 171.6)])
```

To access measurements since a given date:

```python
import datetime

may = datetime.date(2015, 5, 1)

body_fat = client.get_measurements('Body Fat', may)
body_fat
# >> OrderedDict([(datetime.date(2015, 5, 14), 12.8), (datetime.date(2015, 5, 13), 13.1), (datetime.date(2015, 5, 12), 12.7),
#                 (datetime.date(2015, 5, 11), 12.7), (datetime.date(2015, 5, 10), 12.8), (datetime.date(2015, 5, 9), 12.4),
#                 (datetime.date(2015, 5, 8), 12.6),  (datetime.date(2015, 5, 7), 12.7),  (datetime.date(2015, 5, 6), 12.6),
#                 (datetime.date(2015, 5, 5), 12.9),  (datetime.date(2015, 5, 4), 13.0),  (datetime.date(2015, 5, 3), 12.6),
#                 (datetime.date(2015, 5, 2), 12.6),  (datetime.date(2015, 5, 1), 12.7)])
```

To access measurements within a date range:

```python
thisweek = datetime.date(2015, 5, 11)
lastweek = datetime.date(2015, 5, 4)

weight = client.get_measurements('Weight', thisweek, lastweek)
weight
# >> OrderedDict([(datetime.date(2015, 5, 11), 171.6), (datetime.date(2015, 5, 10), 172.4), (datetime.date(2015, 5,9), 170.2),
#                 (datetime.date(2015, 5, 8), 171.0),  (datetime.date(2015, 5, 7), 171.2),  (datetime.date(2015, 5, 6), 170.8),
#                 (datetime.date(2015, 5, 5), 171.8),  (datetime.date(2015, 5, 4), 174.2)])
```

Measurements are returned as ordered dictionaries. The first argument specifies the measurement name, which can be any name listed in the MyFitnessPal [Check-In](http://www.myfitnesspal.com/measurements/check_in/) page. When specifying a date range, the order of the date arguments does not matter.

Nutrition Examples
----

To access nutrition data from the past 30 days:

```python
import myfitnesspal
client = myfitnesspal.Client('my_username', 'my_password')
nutrition = client.get_nutrition(30)
nutrition['Calories']
#nutrition['Fat']
#nutrition['Carbs']
#nutrition['Protein']
# >> OrderedDict([(datetime.date(2015, 7, 3), 2744.0), (datetime.date(2015, 7, 2), 3119.0), (datetime.date(2015, 7, 1), 3099.0),
                  (datetime.date(2015, 6, 30), 3028.0), (datetime.date(2015, 6, 29), 3012.0), (datetime.date(2015, 6, 28), 3002.0),
                  (datetime.date(2015, 6, 27), 3022.0), (datetime.date(2015, 6, 26), 3014.0), (datetime.date(2015, 6, 25), 3006.0),
                  (datetime.date(2015, 6, 24), 3104.0), (datetime.date(2015, 6, 23), 3013.0), (datetime.date(2015, 6, 22), 3002.0),
                  (datetime.date(2015, 6, 21), 3076.0), (datetime.date(2015, 6, 20), 3077.0), (datetime.date(2015, 6, 19), 3005.0),
                  (datetime.date(2015, 6, 18), 3052.0), (datetime.date(2015, 6, 17), 2885.0), (datetime.date(2015, 6, 16), 3023.0),
                  (datetime.date(2015, 6, 15), 2995.0), (datetime.date(2015, 6, 14), 3107.0), (datetime.date(2015, 6, 13), 2990.0),
                  (datetime.date(2015, 6, 12), 3046.0), (datetime.date(2015, 6, 11), 2940.0), (datetime.date(2015, 6, 10), 3876.0),
                  (datetime.date(2015, 6, 9), 3001.0), (datetime.date(2015, 6, 8), 3039.0), (datetime.date(2015, 6, 7), 3116.0),
                  (datetime.date(2015, 6, 6), 2954.0), (datetime.date(2015, 6, 5), 3041.0), (datetime.date(2015, 6, 4), 3101.0)])

```
The nutritional data is returned as a dictionary with the keys Calories, Fat, Carbs, Protein which in turn holds ordered dictionaries that look just like the ordered dicts you get from get_measurements.

Hints
-----

Day objects act as dictionaries:

```python
day.keys()
# >> ['Breakfast', 'Lunch', 'Dinner', 'Snack']
lunch = day['Lunch']
print lunch
# >> [<Generic - Ethiopian - Miser Wat (Red Lentils), 2 cup {'sodium': 508, 'carbohydrates': 76, 'calories': 346, 'fat': 2, 'sugar': 0, 'protein': 12}>,
#     <Injera - Ethiopian Flatbread, 18 " diameter {'sodium': 204, 'carbohydrates': 30, 'calories': 139, 'fat': 1, 'sugar': 0, 'protein': 5}>]
```

Meal objects act as lists:

```python
len(lunch)
# >> 2
miser_wat = lunch[0]
print miser_wat
# >> <Generic - Ethiopian - Miser Wat (Red Lentils), 2 cup {'sodium': 508, 'carbohydrates': 76, 'calories': 346, 'fat': 2, 'sugar': 0, 'protein': 12}>
```

and Entry objects act as dictionaries:

```python
print miser_wat['calories']
# >> 346
```

and, since the measurement units returned are not necessarily very intuitive,
you can enable or disable unit awareness using the `unit_aware` keyword
argument.

```python
client = myfitnesspal.Client('my_username', 'my_password', unit_aware=True)
day = client.get_date(2013, 3, 2)
lunch = day['lunch']
print lunch
# >> [<Generic - Ethiopian - Miser Wat (Red Lentils), 2 cup {'sodium': Weight(mg=508), 'carbohydrates': Weight(g=76), 'calories': Energy(Calorie=346), 'fat': Weight(g=2), 'sugar': Weight(g=0), 'protein': Weight(g=12)}>,
miser_wat = lunch[0]
print miser_wat['calories']
# >> Energy(Calorie=346)
```
