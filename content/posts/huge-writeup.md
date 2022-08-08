---
title: "Huge Writeup"
date: 2022-08-08T10:44:28-05:00
draft: false
tags: crypto
---

## from imaginaryCTF

## Setup
Challenge consists of a challenge file and an output file
Output file includes [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) values c (cipher text), n (modulus), and e (exponent)
In order to decrypt a file encrypted with an RSA encryption, according to wikipedia, one must solve the following equation:
  
`(c^d)modn`
  
`n` and `c` are given but we don't know `d`  

`d` can be found by solving this equation:  
  
`de=1mod(lambda(n))`
  
`e` is known, lambda(n) is not.
  
we can find this value by using an online [euler's totient](https://en.wikipedia.org/wiki/Euler%27s_totient_function) calculator.
  
I used this one : https://comnuan.com/cmnn02/cmnn02005/
  
  
I didn't have the time to figure out how to calculate numbers this large without factor.db type websites
  

After we have this number, we can plug it into the equation and solve. I coded a fast solution below. 

## Code
  

	from Crypto.Util.number import long_to_bytes
	
	n = 257827703087398016057355158654193468564980243813004452658087616586210487667215030370871398983230710387803731676134007721137156696714627083072326445637415561591372586919746606752675050732692230618293581354674196658443898625965651230501721590806987488038754683843111434873697465691139129703835890867256688046172118591
	c = 194667317703687479298989188290833629421904543231503224641743768867721632949682167895699280370759100055314992068135383846690184090232092994595979623784341194946153538127175310278245722299688212621004144260665233469561373125699948009903943019071999887294005117156960295183926108287198987970959145603429337056005819069
	e = 65537
	lambda_n = 238940154401626938037848370480225183045581769211725031481021020007970362783588965693807273855047475931562553093129263532876569906106451113480591159727935347588408235764799039034556484596551260176507450085252133677350349066373844187940165049949179091946213517645837708200090091217192271487206162432000000000000000000
	
	x = lambda_n
	# iterative method to find d
	# xi = lambda_n*i ... i from 1 to inf
	# if xi+1 is divisible by e
	while True:
	    if (x+1)%e == 0:
	        d= (x+1)//e # by luck we can round down if we add 1 to x
	        break
	    x += lambda_n
	
	# this solves the first stated decryption equation and converts the result to string 
	# first by converting the integer to bytes then to string with decode()
	print(long_to_bytes(pow(c,d,n)).decode())
	
	output: 
	ictf{sm4ll_pr1mes_are_n0_n0_9b129443}

