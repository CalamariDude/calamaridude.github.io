---
title:  "Passchecker Writeup"
date:   2022-08-08 17:22:18 -0400
---
## from vsCTF

We were given passchecker2000.py
```
def validate(password: str) -> bool:
    if len(password) != 49:
        return False

    key = ['vs'.join(str(randint(7, 9)) for _ in range(ord(i))) + 'vs' for i in password[::-2]]
    gate = [118, 140, 231, 176, 205, 480, 308, 872, 702, 820, 1034, 1176, 1339, 1232, 1605, 1792, 782, 810, 1197, 880,
            924, 1694, 2185, 2208, 2775]
    if [randint(a, b[0]) for a, b in enumerate(zip(gate, key), 1) if len(b[1]) != 3 * (b[0] + 7 * a) // a]:
        return False

    hammer = {str(a): password[a] + password[a + len(password) // 2] for a in range(1, len(password) // 2, 2)}
    block = b'c3MxLnRkMy57XzUuaE83LjVfOS5faDExLkxfMTMuR0gxNS5fTDE3LjNfMTkuMzEyMS5pMzIz'
    if b64encode(b'.'.join([((b + a).encode()) for a, b in hammer.items()])) != block:
        return False

    return True


if __name__ == "__main__":
    passwd = input('Please validate your ID using your password\n> ')
    if validate(passwd):
        print('Access Granted: You now have gained access to the View Source Flag Vault!')
    else:
        print('Access Denied :(')
```


the main function takes in a password and determines if it is valid. We want to get a valid password since that would be the flag.
The validate function has three conditions that we must pass in order to get the flag.

1st one is a simple length check:
```if len(password) != 49:
    return False
```
I commented this out so I could examine the workings of the rest of the challenge

the second obstacle for the password is a permutation that takes the string provided, goes from back to front, skipping every other letter, and converting to some kind of cipher.
```key = ['vs'.join(str(randint(7, 9)) for _ in range(ord(i))) + 'vs' for i in password[::-2]]```
Every other letter from back to front has its `ord` or ascii number evaluated and used to create a list of string, where each string in the list contains the sequence of `[7,8,9]vs`, `n` times where n equals the `ord` number of the letter being evaluated.
A string like `a!!` would turn into an array of lenght two, the first element containing ord('!') = 33 sequences of `[7,8,9]vs` ... ie `7vs8vs7vs9vs7vs8vs...8vs', the second will have ord('a')= 97 sequences of the pattern.
The flag information is given in a form of numbers:
```gate = [118, 140, 231, 176, 205, 480, 308, 872, 702, 820, 1034, 1176, 1339, 1232, 1605, 1792, 782, 810, 1197, 880,
            924, 1694, 2185, 2208, 2775]

 if [randint(a, b[0]) for a, b in enumerate(zip(gate, key), 1) if len(b[1]) != 3 * (b[0] + 7 * a) // a]:
     return False```
this gooky equation which basically says for each element in the gate, add 7 * current index (starting at 1 not 0) and divide the sum by the current index then convert the product to a character (int to chr)

Copying the gate list to a separate python file, we can then compute the flag with this: equation
```a = ""
counter = 1
for g in gate:
    character = chr(int((g + 7*counter)//counter))
    counter +=1
    a+=character
print(a) #v_c_f_T_3_3_F_4_5_w_r___n_i_e_Y_U_t_3_W_0_3_T_M_}
```

This lets us know every other character in the flag. To get the other `half` of the flag we must go through this challenge:

```hammer = {str(a): password[a] + password[a + len(password) // 2] for a in range(1, len(password) // 2, 2)}
    block = b'c3MxLnRkMy57XzUuaE83LjVfOS5faDExLkxfMTMuR0gxNS5fTDE3LjNfMTkuMzEyMS5pMzIz'
    if b64encode(b'.'.join([((b + a).encode()) for a, b in hammer.items()])) != block:
        return False
```
this basically takes a block of bytes and encodes indexes and values of the remaining letters. To decode the block we do
```rush = [a.decode() for a in b64decode(b'c3MxLnRkMy57XzUuaE83LjVfOS5faDExLkxfMTMuR0gxNS5fTDE3LjNfMTkuMzEyMS5pMzIz').split(b'.')]
print(rush) # ['ss1', 'td3', '{_5', 'hO7', '5_9', '_h11', 'L_13', 'GH15', '_L17', '3_19', '3121', 'i323']```

each string has 3 pieces of information encoded like so:
`1st character: character 1`
`2nd character: character 2`
`3rd character onwards: position of the 1st character `

One thing we also seee from the code is that the 2nd character position is always 24 + position of the 1st character1

so we can decrypt this with: 

```for a in rush:
    print(a)
    index = int(a[2:])
    index2 = int(a[2:])+24
    value1 = a[0]
    value2 = a[1]
    print(index, index2, value1, value2)
    current_flag[index] = value1
    current_flag[index2] = value2
print(current_flag) # vsctf{Th353_FL4G5_w3r3_inside_YOU_th3_WH0L3_T1M3}
```
we don't have to rerun the program with the encrypted version of the flag since we know what the decyrpted version is.

