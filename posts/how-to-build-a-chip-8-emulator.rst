.. title: How to build a CHIP-8 emulator?
.. slug: how-to-build-a-chip-8-emulator
.. date: 2018-08-03 10:14:04 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

So, a couple of weeks ago, I came across this post_ which talks about making a Game Boy emulator. In the introduction, 
CHIP-8 was introduced as a first project to understand emulation. I decided to build one myself, so I have decided to reuse some
code I had for the `Space Invaders`_. 

The CHIP-8 emulation is not very hard, as suggested author in the introduction and all the
required instructions to implement can be found in the Wikipedia_ page. Also I had some help about getting started with this guide_.

In this guide, I will explain how to build one from scratch. 

Note: I would not be covering how to get the keys and drawing graphics, that would depend on the framework that you use, 
i.e. SDL, GLFW (for my case), GLUT. Also I am writing this guide in D.

What is CHIP-8
--------------
It's actually a virtual machine created by Joseph Weisbecker in the mid-1970s. As such, it is actually like a mini computer that
can run instructions and output useful results onto the screen and beep sound (more on this later).

To start, we have to understand the specifications of this (virtual) machine.

CHIP-8 specifications
---------------------
- CPU - 2-bytes (16-bit instructions)

.. code-block:: d

    ushort pc;

- Opcode (instructions) - There are 35 opcodes_ which are all 2-bytes (16-bits) long and stored big-endian.
    
.. code-block:: d

    ushort opcode;

- Registers - 16 1-byte (8-bit) registers, named from V0 to VF and a 2-bytes I address register.

.. code-block:: d

    char[16] vreg;
    ushort ireg;

- Memory - 4K memory, where all memory locations are 1-byte (8-bit where the name is from).

.. code-block:: d

    char[4096] memory;

- Stack - The stack is only used to store return addresses when subroutines are called, supports up to 16 levels of nested calls.

.. code-block:: d

    ushort[16] stack;
    ushort stackPointer;

- Timers - Delay and Sound timers

.. code-block:: d

    // Interrupts and hardware registers, CHIP-8 has none
    // but there are two timer that count ay 60 Hz,
    // will count down if set above zero.
    char delayTimer;

    // Buzzer will sound when sound timer reaches 0.
    char soundTimer;

- Screens - 64 x 32 pixels, color is monochrome. We will eventually have our Graphics API to copy this buffer and draw them on screen.

.. code-block:: d

    // Screen size: 64 x 32 = 2048 px
    const size_t screenWidth = 64;
    const size_t screenHeight = 32;
    char[screenWidth * screenHeight] screen;

CPU
---
In one cycle of this CPU, we need to Fetch, Decode, Execute, Writeback (FDEW) as well as update the timers in the machine.

.. code-block:: d

    void CPUCycle()
    {
        // Fetch opcode
        // Decode opcode, execute, writeback, advance PC if needed
        // Update timers.
    }

Firstly, we will need to implement the fetching of opcode. 

Fetching
========
The opcode are stored in memory 2-bytes by 2-bytes, therefore when fetching our opcode, we fetch
from the PC and PC + 1 and combine them together to form our opcode.

.. code-block:: d

    // Fetch opcode
    opcode = memory[pc] << 8 | memory[pc + 1];

Decode, Execute, Writeback
==========================
After we get the opcode, we will now decipher what the instructions mean. Looking at the opcodes_ table, we will see that
the opcode are arranged nicely from 0XXX, 1XXX ... FXXX.

.. code-block:: d

    // Decode opcode, execute, writeback, advance PC if needed
    switch (opcode & 0xF000) // Check first bit of opcode
    {
        // Handle 0XXX
        // Handle 1XXX
        //      .
        //      .
        // Handle FXXX
        default: break;
    }

Note: Do not forget to increment our PC once we finished executing.

.. code-block:: d

    // Since we fetch every 2-byte, we will skip by that
    pc += 2;

Let's look at a simple example at opcode **1NNN**. From the wikipedia page, it says that NNN is the address to jump to. We will
extract NNN from the opcode and jump there.

.. code-block:: d
    
    // Handle 1XXX
    case 0x1000:
        const auto NNN = opcode & 0x0FFF;
        pc = NNN;
        break;

Note: In this case, we do not increment our PC because we are doing a jump to that location and hence no need to increment our PC.

Similarly, let's take a look at opcode **2NNN**. This is similar to the above instructions, but we are doing a call instead of jump,
hence we need to store the return address of the current PC and jump to the new location, when the routine encounters a return,
we will jump back to this current location.

.. code-block:: d

    // Handle 2XXX
    case 0x2000:
        // Calls subroutine at NNN
        const auto NNN = opcode & 0x0FFF;

        // Push current pc onto the stack
        stack[stackPointer++] = pc;
        pc = NNN;
        break;

Lastly, we will take a look at a normal operations that involves the register. Note: we will increment the PC by 2 no matter what
the result is. As per this opcode, it will advance the PC if the VX register is the same as the address NN.

.. code-block:: d

    // Handle 3XXX
    case 0x3000:
        // Skips the next instruction if VX equals NN.
        const auto X = (opcode >> 8) & 0x000F;
        const auto NN = opcode & 0x00FF;

        // Condition for this opcode is to skip next instruction 
        // if condtion is fulfilled
        if (V[X] == NN)
            pc += 2;
        
        // Increment PC after executing this instruction
        pc += 2;
        break;

Next, we will implement the timers. For this design, we will be printing the word BEEP however the real machine will
play a beep sound.

.. code-block:: d

    // Update timers.
    if (delayTimer > 0)
      --delayTimer;

    if (soundTimer > 0)
    {
      if (soundTimer == 1)
        writeln("BEEP!");
      --soundTimer;
    }

That will be what your CHIP-8 CPU will be doing in one cycle. For more examples of the opcode, 
please check out my CHIP-8_ implementation.

Keys
----
The mapping is as follows, but you can do anything you want as long as the keypad mapping is correct.

+---------------+--+---------------+  
|   Keyboard    |=>|    Keypad     |
+===+===+===+===+==+===+===+===+===+
| 1 | 2 | 3 | 4 |=>| 1 | 2 | 3 | C |
+---+---+---+---+--+---+---+---+---+
| Q | W | E | R |=>| 4 | 5 | 6 | D |
+---+---+---+---+--+---+---+---+---+
| A | D | D | F |=>| 7 | 8 | 9 | E |
+---+---+---+---+--+---+---+---+---+
| Z | X | C | V |=>| A | 0 | B | F |
+---+---+---+---+--+---+---+---+---+

Fonts
-----
This are the required font set data for drawing the proper fonts.

.. code-block:: d

    // All the required fonts
    char[80] fontset =
    [ 
        0xF0, 0x90, 0x90, 0x90, 0xF0, // 0
        0x20, 0x60, 0x20, 0x20, 0x70, // 1
        0xF0, 0x10, 0xF0, 0x80, 0xF0, // 2
        0xF0, 0x10, 0xF0, 0x10, 0xF0, // 3
        0x90, 0x90, 0xF0, 0x10, 0x10, // 4
        0xF0, 0x80, 0xF0, 0x10, 0xF0, // 5
        0xF0, 0x80, 0xF0, 0x90, 0xF0, // 6
        0xF0, 0x10, 0x20, 0x40, 0x40, // 7
        0xF0, 0x90, 0xF0, 0x90, 0xF0, // 8
        0xF0, 0x90, 0xF0, 0x10, 0xF0, // 9
        0xF0, 0x90, 0xF0, 0x90, 0x90, // A
        0xE0, 0x90, 0xE0, 0x90, 0xE0, // B
        0xF0, 0x80, 0x80, 0x80, 0xF0, // C
        0xE0, 0x90, 0x90, 0x90, 0xE0, // D
        0xF0, 0x80, 0xF0, 0x80, 0xF0, // E
        0xF0, 0x80, 0xF0, 0x80, 0x80  // F
    ];

These represents the sprite data, one sprite is 4 pixel wide and 5 pixel high. Note the following example, 
0xF0 truncated to 0xF because 1 sprite is 4 pixel wide (4-bits) instead.

+-----------------------+-----------------------+  
|       0               |       1               |
+===+===+===+===+=======+===+===+===+===+=======+
| 1 | 1 | 1 | 1 |   0xF |   |   | 1 |   |   0x2 |
+---+---+---+---+-------+---+---+---+---+-------+
| 1 |   |   | 1 |   0x9 |   | 1 | 1 |   |   0x6 |
+---+---+---+---+-------+---+---+---+---+-------+
| 1 |   |   | 1 |   0x9 |   |   | 1 |   |   0x2 |
+---+---+---+---+-------+---+---+---+---+-------+
| 1 |   |   | 1 |   0x9 |   |   | 1 |   |   0x2 |
+---+---+---+---+-------+---+---+---+---+-------+
| 1 | 1 | 1 | 1 |   0xF |   | 1 | 1 | 1 |   0x7 |
+---+---+---+---+-------+---+---+---+---+-------+

Fonts are to be loaded at initialization into memory

.. code-block:: d

    // Emulator initialization
    foreach (i; 0 .. 80)
        memory[i] = fontset[i];

Conclusion
----------

That's it for this guide. I hope you enjoyed it. If you are ever stuck, there are tons of resources out there, or you can head on over to my
CHIP-8_ implementation to take a look. Have fun hacking.

I think this is a good exercise to do for anybody learning about computers, especially the stuff on program counter and registers.
I strongly encourage anybody to build a CHIP-8 virtual machine.

.. _post: https://cturt.github.io/cinoop.html
.. _Space Invaders: https://github.com/zgoh/d_space_invaders
.. _Wikipedia: https://en.wikipedia.org/wiki/CHIP-8
.. _guide: http://www.multigesture.net/articles/how-to-write-an-emulator-chip-8-interpreter/
.. _opcodes: https://en.wikipedia.org/wiki/CHIP-8#Opcode_table
.. _CHIP-8: https://github.com/zgoh/d_chip8