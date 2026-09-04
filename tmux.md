# learn tmux 

tmux is a terminal multipexer . this is wery usefull tool for a linux user or a developer who use linux to create a app or .... useing linux .

tmux give os the feture that the normal terminal don't . the other feture of the tmux is costomising you can costome the terminals plaises by your use or you can use the windows (not windows form Microsoft) or ... fetuer

tmux has a **prefix** key whitch is ***Ctrl + B*** at here i am watching a youtube corse from **learn linux tv** you can see it your self if you dont feel to read this doc . i'll go part by part to read esier and acsess the topic faster

## lplite the window (vertical and horisental)

to splite the window to 2 sepeated terminal you can use :

```keys
prefix + "
prefix + %
```

this is way to mangement your window and also you can resize your teminals by :

```keys
prefix + holding Ctrl + arrow key
```
to management the terminals in the window.

## session management

you can detach your tmux session so you can go to the normal terminal and use it for what you want :)
ofcorse you can attach to your terminal to go back to tmux session and continue your task or ... .

you can detache your session useing the :

```keys
prefix + d 
``` 

and to attach to session you can use :

```bash
tmux attach 
```

what if you have multy sessions in your PC lets sey you have 3 sessions [0 , 1 , 2 , 3] and you need to go to session 0 but you are now in session 3 , you can close all the sessions to rietch to your target (what i was do to change betwin the sessions) or you can do it directly to retch the session without closeing other sessions 

at this case you can use the commant 

```bash
tmux attach -t TARGET-NUMBER-IN-LIST-SESSIONS
```



## create a new window

a window as its name is a new workspace in the tmux and as for as i know you can create new window by two way you can use the command and you can use the short-key to create a new window.

the command way open the tmux than in the tmux terminal type this command :

```bash 
tmux new-window
```

this is the short-key way : open the tmux in your terminal than use :

```keys
prefix + c
```

to change the window in tmux you can use the :

```keys
prefix + p
prefix + n 
```

***p*** for left and ***n*** for righte . 
if you have create a lot of windows you can use the below keys to get a full view and see what is happening in eatch window

```keys
prefix + s
```

also if you whant to close a window in tmux use 

```keys
prefix + &
```

if you want to rename the window you can do it useing 

```keys
prefix + ,
```

or you can use comment :

```bash
tmux rename-window new-name
```

## 


