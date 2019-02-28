
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

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Installing
==========

To install the regular library without async support, just run the following command:

.. code-block:: none

  python3 -m pip install -U akinator.py

Otherwise, to get asynchronous support, do:

.. code-block:: none

  python3 -m pip install -U akinator.py[async]

To get async support plus faster performance (via the ``aiodns`` and ``cchardet`` libraries), do:

.. code-block:: none

  python3 -m pip install -U akinator.py[fast_async]

Quick Examples
==============

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

Documentation
=============

Because this library is relatively simple and only has a few functions to keep track of, all the documentation is gonna go here in the README, instead of on a separate site like `readthedocs.io <https://readthedocs.org/>`_ or something.

The async version of this library works almost exactly the same as the regular, non-async one. Both have the same classes, names of functions, etc. Any differences will be noted.

To use the regular version of akinator.py, type ``import akinator`` at the top of your program. To use the one with async support, type ``import akinator.async_aki`` OR ``from akinator.async_aki import Akinator``


*class* Akinator()
------------------

Sample placeholder text

Functions
^^^^^^^^^

**Akinator.start_game(language=None)**

More placeholder text

**Akinator.answer(ans)**

Even more sample text stuff

Variables
^^^^^^^^^

Akinator.server
  Even more random placeholder text

Akinator.session
  Lorem ipsum something
