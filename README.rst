
===========
akinator.py
===========

**An API wrapper for the online game, Akinator, written in Python**

.. image:: https://img.shields.io/badge/pypi-v1.0.0-blue.svg
    :target: https://pypi.python.org/pypi/akinator.py/

.. image:: https://img.shields.io/badge/python-3.5%20%7C%203.6-yellow.svg
    :target: https://pypi.python.org/pypi/akinator.py/

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Copyright (c) 2019 NinjaSnail1080

Licensed under the MIT License (see ``LICENSE.txt`` for details).

`Akinator.com <https://www.akinator.com>`_ is an online game where you think of a character, real or fiction, and by asking you questions the site will try to guess who you're thinking of. This library allows for easy access to the Akinator API and makes writing programs that use it much simpler.

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**********
Installing
**********

To install the regular library without async support, just run the following command:

.. code-block:: none

  python3 -m pip install -U akinator.py

Otherwise, to get asynchronous support, do:

.. code-block:: none

  python3 -m pip install -U akinator.py[async]

To get async support plus faster performance (via the ``aiodns`` and ``cchardet`` libraries), do:

.. code-block:: none

  python3 -m pip install -U akinator.py[fast_async]

**************
Quick Examples
**************

Here's a quick little example of the library being used to make a simple, text-based Akinator game:

.. code-block:: python

  import akinator
  aki = akinator.Akinator()

  def main():
      q = aki.start_game()
      while aki.progression <= 85:
          a = input(q + "\n\t")
          if a == "b":
              try:
                  q = aki.back()
              except akinator.CantGoBackAnyFurther:
                  pass
          else:
              q = aki.answer(a)
      win = aki.win()
      correct = input(f"It's {aki.name} ({aki.description})! Was I correct?\n{aki.picture}\n\t")
      if correct.lower() == "yes" or correct.lower() == "y":
          print("Yay\n")
      else:
          print("Oof\n")

  main()

Here's the same game as above, but using the async version of the library instead:

.. code-block:: python

  from akinator.async_aki import Akinator
  import akinator
  import asyncio

  loop = asyncio.get_event_loop()
  aki = Akinator(loop)

  async def main():
      q = await aki.start_game()
      while aki.progression <= 85:
          a = input(q + "\n\t")
          if a == "b":
              try:
                  q = await aki.back()
              except akinator.CantGoBackAnyFurther:
                  pass
          else:
              q = await aki.answer(a)
      win = await aki.win()
      correct = input(f"It's {aki.name} ({aki.description})! Was I correct?\n{aki.picture}\n\t")
      if correct.lower() == "yes" or correct.lower() == "y":
          print("Yay\n")
      else:
          print("Oof\n")

  loop.run_until_complete(main())
  loop.close()

*************
Documentation
*************

Because this library is relatively simple and only has a few functions to keep track of, all the documentation is going to go here in the README, instead of on a separate site like `readthedocs.io <https://readthedocs.org/>`_ or something.

The async version of this library works almost exactly the same as the regular, non-async one. Both have the same classes, names of functions, etc. Any differences will be noted.

To use the regular version of akinator.py, type ``import akinator`` at the top of your program. To use the one with async support, type ``import akinator.async_aki`` or ``from akinator.async_aki import Akinator``.

*class* Akinator()
==================

A class that represents an Akinator game.

The first thing you want to do after calling an instance of this class is to call ``Akinator.start_game()``.

In the aysnc version, this class also has an optional parameter called ``loop``, which can be either left as None or assigned to an asyncio event loop.

Functions
=========

**Note**: In the async version, all the below functions are coroutines and must be awaited 

Akinator.start_game(language=None)
  Start an Akinator game. Run this function first before the others. Returns a string containing the first question

  The ``language`` parameter can be left as None for English, the default language, or it can be set to one of these:

  - ``en``: English
  - ``en2``: Second English server. Use if the main one is down
  - ``ar``: Arabic
  - ``cn``: Chinese
  - ``de``: German
  - ``es``: Spanish
  - ``fr``: French
  - ``fr2``: Second French server. Use if the main one is down
  - ``il``: Hebrew
  - ``it``: Italian
  - ``jp``: Japanese
  - ``kr``: Korean
  - ``nl``: Dutch
  - ``pl``: Polish
  - ``pt``: Portuguese
  - ``ru``: Russian
  - ``tr``: Turkish

  You can also put the name of the language spelled out, like ``spanish``, ``korean``, etc.

Akinator.answer(ans)
  Answer the current question, which you can find with ``Akinator.question``. Returns a string containing the next question

  The ``ans`` parameter must be one of these:

  - `yes` or ``y`` or ``0`` for YES
  - ``no`` or ``n`` or ``1`` for NO
  - ``i`` or ``idk`` or ``i dont know`` or ``i don't know`` or ``2`` for I DON'T KNOW
  - ``probably`` or ``p`` or ``3`` for PROBABLY
  - ``probably not`` or ``pn`` or ``4`` for PROBABLY NOT

Akinator.back()
  Goes back to the previous question. Returns a string containing that question

  If you're on the first question and you try to go back again, the CantGoBackAnyFurther exception will be raised

Akinator.win()
  Get Aki's first guess for who the person you're thinking of is based on your answers to the questions.

  This function defines 3 new variables:

  - ``Akinator.name``: The name of the person Aki guessed
  - ``Akinator.description``: A short description of that person
  - ``Akinator.picture``: A direct link to an image of the person

  This function will also return a dictionary containing the above values plus some additional ones. Here's an example of what the dict looks like:

  .. code-block:: json

    {'absolute_picture_path': 'https://photos.clarinea.fr/BL_25_en/600/partenaire/q/2367495__1923001285.jpg',
     'description': 'Entrepreneur',
     'flag_photo': 0,
     'id': '28146',
     'id_base': '2367495',
     'minibase_addable': '0',
     'name': 'Elon Musk',
     'picture_path': 'partenaire/q/2367495__1923001285.jpg',
     'proba': '0.937118',
     'pseudo': 'Rob',
     'ranking': '390',
     'relative_id': '-1',
     'valide_contrainte': '1'}

  It's recommended that you call this function when Aki's progression is above 85%. You can get his current progression via ``Akinator.progression``

Variables
=========

These variables contain important information about the Akinator game

Akinator.server
  The server this Akinator game is using. Depends on what you put for the language param in ``Akinator.start_game()`` (e.g., ``"srv11.akinator.com:9152"``, ``"srv11.akinator.com:9150"``, etc.)

Akinator.session
  A number, usually in between 0 and 100, that represents the game's session.

Akinator.signature
  A usually 9 or 10 digit number that represents the game's signature

Akinator.question
  The current question that Akinator is asking the user. Examples of questions asked by Aki include: ``Is your character's gender female?``, ``Is your character more than 40 years old?``, ``Does your character create music?``, ``Is your character real?``, ``Is your character from a TV series?``, etc.

Akinator.progression
  A number that represents a percentage showing how close Aki thinks he is to guessing your character.

Akinator.step
  Lorem ipsum something

Exceptions
==========

WIP