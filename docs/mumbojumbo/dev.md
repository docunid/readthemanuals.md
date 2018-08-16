# device files

- /dev/full: always full device, `echo "Hello world" > /dev/full` returns "No space left on device"
- /dev/mapper: LVM logical volumes
- /dev/null: blank file with size zero, `echo "Hello World" > /dev/null` sends bytes into the void
- /dev/random: returns random bytes ad infinitum
- /dev/shm: shared (virutal) memory usually mounted as tmpfs filesystem `mount -t tmpfs -o size=5G,nr_inodes=5k,mode=700 tmpfs /disk2/tmpfs`
- /dev/stdin: standard input of current porcess
- /dev/tty: the terminal to which the current process is connected
- /dev/urandom: returns random bytes ad infinitum, a file with 4K of random bytes `cat /dev/urandom > /tmp/4krandom`
- /dev/zero: returns null bytes ad infinitum, 1MB of null characters `dd if=/dev/zero of=foobar count=1024 bs=1024`

## fun and games

- fork bomb `:(){ :|: & };:` (Create a function that calls itself twice every call and doesn't have any way to terminate itself. It will keep doubling up until you run out of system resources.)
- never ending proces 'cat /dev/zero' (/dev/zero returns infinitely 0-bytes. The process will never end.)
