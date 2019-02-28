
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

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Installing
==========

To install the regular library without async support, just run the following command:

.. code-block::

  python3 -m pip install -U akinator.py

Otherwise, to get asynchronous support, do:

.. code-block::

  python3 -m pip install -U akinator.py[async]

To get async support plus better performance (via the ``aiodns`` and ``cchardet`` libraries), do:

.. code-block::

  python3 -m pip install -U akinator.py[fast_async]

"""""""""""""""""""""""""""""""""""""""""""""""""""""

Quick Examples
==============

Here's a quick little example of the library being used to make a simple, text-based Akinator game:

.. code-block:: python

  import akinator

  aki = akinator.Akinator()


  def main():
      q = aki.start_game()
      while True:
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
              break
          else:
              print("Oof\n")
              break


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
      while True:
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
              break
          else:
              print("Oof\n")
              break


  loop.run_until_complete(main())
  loop.close()

WIP