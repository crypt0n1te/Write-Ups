## Briefing: 
`Mr.Wolf was doing some work and he accidentally deleted the important file can you help him and read the file?
ssh ctf@findme.darkarmy.xyz -p 10000 password: wolfie`

Running `ps aux` to see the running processes showed us that the command `tail -f /home/wolf1/pass` was running at PID 10. However in the /home/wolf1 directory, this file was not to be found. 

After googling how to view the contents of a background process I ran the command `cat /proc/10/fd/*` and got `mysecondpassword123`. 

Since there was a `wolf2` I figured this was the password for wolf2 so running `su wolf2` and inputting this as the password means we are now wolf2. List the files and get the flag.

### Flag:  
`darkCTF{w0ahh_n1c3_w0rk!!!}`

