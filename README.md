# ARP--Second-Assignment

## Description

A logfile was created for each process to control what happens.

### MASTER
This process handles the spawning of the two processes and also their closure. Two pipes are used to obtain the pids of the processes, which are useful for closing them. In addition, the SIGINT signal is also handled in order to close the master and child processes in the best possible way.

### PROCESS A
In this process, the shared memory is created and the semaphore required to protect it is created and initialised. After opening and closing a pipe for sending the pid to the master, three functions are used. 
The first used is the delete function for cleaning the bitmap; 
the second used is "bmp_circle" for drawing a circle in the centre of the bitmap;
the third is 'static_conversion', through which the shared memory is accessed and the bitmap data is passed to it. Obviously, this function is protected by a semaphore so as to prevent the second process from attempting to access this memory.
Then we enter an endless while loop in which, if the "P" button is pressed, a screenshot of the bitmap will be taken, if one of the four arrows in the keyboard is used, then via "move_circle" and "draw_circle" the drawing on the first interface will be moved and drawn. Finally, the three functions described above will be used in the same order, the only difference being that they will be passed to the "bmp_circle" function the co-ordinates of the design on the interface.
The closure is handled with a sigaction, via the management of the SIGTERM and SIGINT signals.
This process handles the closing of all resources.

N.B. Thecoordinates are multiplied by 20 to allow their use in the bitmap.

### PROCESS B
This process is very similar to process A in some respects, but here 'ftruncate' is not used and the semaphore is simply opened and not initialised, because it is all done by process A.
A pipe is also opened and closed here to send the pid to the master.
Before the while loop, the 'delete' function, already described above, is used. 
Within the while loop, I implemented the possibility of taking a screenshot of the bitmap of this process using the 's' key on the keyboard.
Following this is the 'conversion' function protected by a semaphore.
In this function, access is made to the shared memory in order to take the data and insert it into a bitmap.
Following this function is the 'get_center' function, which obtains the coordinates of the centre of the circle in the bitmap. It is possible to obtain the co-ordinates by knowing the radius of the circle. Finally, through the use of "mvdacch", zeros will be shown on the screen that will track the movement of the drawing in the first interface.
Closure is handled in the same way as process A.
N.B. The coordinates obtained are divided by 20 to allow them to be used in the interface. 

## Installing
### Install Libbitmap

download from https://github.com/draekko/libbitmap

navigate to the root directory of the downloaded repo and type ./configure

type make

run make install

### Install ncurses

execute sudo apt-get install libncurses-dev

execute install.src

## Run

execute run.src

## User guide

There are two interfaces. In the first, a small drawing appears which can be moved using the arrows on the keyboard. In addition, clicking on the box where there is a 'P' will provide a screenshot of the bitmap.
In the second interface, zeros will appear which will trace the movements of the drawing in the first interface, and by clicking the "s" key on the keyboard it will be possible to obtain a screenshot of the bitmap of process B.
