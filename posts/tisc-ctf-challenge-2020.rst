.. title: TISC CTF challenge 2020
.. slug: tisc-ctf-challenge-2020
.. date: 2020-09-08 19:59:39 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

TISC CTF challenge 2020 is a CTF challenge organized by the Centre for Strategic Infocomm
Technologies (CSIT). There a few stages in the challenge which I will talk about, and as I did not
complete the whole CTF and it was a 48 hour event, I did not have a chance to go back and complete
it. I am still figuring out stage 3 on my own time. Do let me know if you completed it, I am
curious about solving it.

Stages
======

- Stage 1 ( Cracking zip challenge )
- Stage 2 ( Finding the public key in binary )
- Stage 3 ( Decrypting file challenge )

Stage 1
-------

In this stage, there is a password protected zip file with instructions to extract the content
buried inside the compressed file. Apparently the file was compressed a few times by different
programs and the final zip file was password protected. The only hint about the password is that,
it is a password of length 6 and only contains hexadecimal string (meaning from 000000 - FFFFFF).

Cracking the zip file
#####################

This part is fairly straightforward with password cracking tool, fcrackzip.

.. code-block:: bash
   
   $ fcrackzip -c 1:abcdefABCDEF -l 6 -u file.zip

This will run fairly quickly and will get us the password we need to decompress the file. The file 
content inside is a file name temp.mess. According to the description given, the file is compressed/
encoded a few times. To verify this, run the file command on the temp.mess file. Doing this will
reveal it as a gz file.

.. code-block:: bash
   
   $ file temp.mess
   temp.mess: gz ....

Decompressing and decoding
##########################

After we decompress the file, we realize that the output of the file is another xz compressed file,
we will decompress the file a few more times and we will
see that the output will be compressed with one of the following,

    - bzip
    - gzip
    - xz
    - zlib

and the following encoding being used,

    - base64
    - hexadecimal

Basically I wrote a python script to read the file using libmagic to determine their file type,
if it is any of the above, I will either decode it or decompress it using the corresponding library
in python. The end result will be a json file with the flag.

Stage 2
-------

In stage 2, a ransomware binary is shared and the objective is to find the public key used. The
flag can then be obtained by getting the sha256 hash of the public key. The hint in the challenge
is that the public key is in the form LS0. If you are unfamiliar like I was initially, the base64
encoding of the public key is in the form LS0....

Now that the string to find is given, we can start to hunt for the keys.

Analyze the binary
##################

Running file command on the binary indicates that it is an ELF 64 binary built with Go compiler. 
It is also unstripped which is useful for our analysis.

Unpack the binary
#################

When analyzing the binary, make sure it is unpacked before doing in depth analysis. I made the
mistake of analyzing with strings but I managed to spot the string "UPX" that made me remember that
binary can be packed. Therefore you can use UPX here to unpack it.

.. code-block:: bash
   
   $ upx -d binary

After which I ran strings again to check, unfortunately in Go, the strings are not null terminated
therefore strings might not give any useful result.

Analyze binary in Ghidra
########################

After I realized strings is not as useful, I loaded the binary in Ghidra and have a look at it
through some static analysis. I note down the functions of interest and their memory address. In Go,
the main is main_main, and the init is in main_init. Go uses a stack calling convention as opposed
to the usual register based x86 calling convention. This made the analysis a bit harder for me as I
learned afterwards.

Anyways there are some functions of interest to note. One of which is the DecodeString function
that is called in main_main.

Getting the flag with GDB
#########################

Be careful when dealing with the sample, although it is not of a malicious intent, it could cause
potential nusiance and thus I would advice to run the binary in a VM.

I start GDB with the binary and immediately set the breakpoint at the DecodeString function. 
Below is how I set the breakpoint and check the function.

.. code-block:: bash

   (GDB) b *0x12345    # Set breakpoint at address 0x12345
   (GDB) r             # Run the program
   (GDB) si            # Step into
   (GDB) info args     # Check the func args passed

Here we should see the execution enters the function and when it enters the function, info args
command will print the args passed into the function. Here we will see that one of the argument is
a string of this form "LS0...."

Here we can copy and paste the string and test if the string is really what we are looking for.

.. code-block:: bash

   $ echo LS0.... | base64 -d

This should print ---Begin ..... which marks the public key in pem encoding. To get the flag, we can
do the following

.. code-block:: bash

   $ sha256 -s LS0...

This will be the flag for stage 2.

Conclusion
----------

Unfortunately I did not make it to stage 3. I did some offline analysis on my own but I could not 
figure out what the binary is doing. I hope one day I will post about the analysis of stage 3. 
That is all for my TISC 2020 CTF. It have been a nice experience for me.
