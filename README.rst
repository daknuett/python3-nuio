Python3 NUIO
************


nuio is an attempt to add a standardized
and easy-to-use user input/output interface for python.

The goal is to provide a set of classes with *print-alike-operating* methods
and a few other methods for markup & co to deal with user input/output.

Features
========

Currently nuio is able to eal with POSIX Terminals.
The corresponding classes are in ``nuio.input.terminal`` and ``nio.output.terminal``.

Philosopy
=========

nuio tries to keep stuff as simple as possible,
but it requires python3.5.1+ language features.

Any blocking user-input is done using async features, this
will become handy in future.


Plans
=====

The following features should be added:

- curses based I/O
- an AJAX based I/O system
- non-POSIX terminals
- fancy frames for BlockOutput


Example
=======

This is a simple example for nuio:

::

	#!/usr/bin/python3

	from nuio.input.terminal import POSIXTerminalInput
	from nuio.output.terminal import TerminalOutput, ESCAPES, TerminalBlockOutput
	import asyncio, shutil


	async def main():
		width, height = shutil.get_terminal_size()

		# an output device
		out = TerminalBlockOutput(height - 10, width)
		out.print("This is a test")

		# input devices require an output device to handle prompts.
		inp = POSIXTerminalInput(out)

		data = await inp.input_int(prompt = "enter an integer > ")

		# You are able to use a bunch of ANSI escape sequences.
		# Usually you should use nuio.output.terminal.colors instead,
		# this will be compatible with other output devices.

		out.print_colored(ESCAPES["underline"], "you entered:", data)

		data = await inp.input_int(prompt = "enter an integer [4-100] > ",
				range = range(4, 100))
		out.print_colored(ESCAPES["fg_red"], "you entered:", data)

		data = await inp.input_float(prompt = "enter an float [4-100] > ",
				range = range(4, 100))
		out.print_colored(ESCAPES["bg_cyan"] + ESCAPES["fg_black"], "you entered:", data)



	# Usually you should make the input async,
	# but to simplify this the complete main function is async.
	loop = asyncio.get_event_loop()
	loop.run_until_complete(main())
	loop.close()
