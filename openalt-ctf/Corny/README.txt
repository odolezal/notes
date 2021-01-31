Source: https://medium.com/the-kickstarter/steganography-on-kali-using-steghide-7dfd3293f3fa

root@kali:~/openalt-ctf# steghide extract -sf corny.jpg 
Enter passphrase: 
wrote extracted data to "flag.txt".
root@kali:~/openalt-ctf# ls
corny.jpg  flag.txt
root@kali:~/openalt-ctf# cat flag.txt 
OA{Wh0s_Th3_C0rnY_Un1c0Rn}

