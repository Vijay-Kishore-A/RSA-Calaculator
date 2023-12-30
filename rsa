import random
from sympy import isprime


def mod_inverse(t, e, a=1, b=0, c=-1):
    na, nb, nc = b, a - b * c, t // e
    mod = t % e
    return na - nb * nc if mod == 1 else mod_inverse(e, mod, na, nb, nc)


def find_d(t, key):
    d = mod_inverse(t, key)
    if d < 0:
        d = t + d
    print("Your Private-key(d) is:", d)
    return d


def gcd(x, y):
    while y != 0:
        x, y = y, x % y
    return x == 1


def find_e(t):
    print("Some value for \"e\" are :", end=" ")
    l = 2
    u = t - 1
    i = 0
    while i < 10:
        random_e = random.randint(l, u)
        if isprime(random_e) and gcd(random_e, t):
            print(random_e, end=" ")
            i += 1


def modulus_power(b, e, mod):
    p = 1
    b = b % mod
    while e > 0:
        if e % 2 == 1:
            p = (p * b) % mod
        b = (b * b) % mod
        e //= 2
    return p


def encryption(n, puk, ptext):
    r = []
    chunks = [ptext[i:i + 3] for i in range(0, len(ptext), 3)]
    print(chunks)
    num_chunks = []
    for word in chunks:
        num_chunks.append(int(word.encode('utf-8').hex(), 16))
    print(num_chunks)
    for text_num in num_chunks:
        x = modulus_power(text_num, puk, n)
        r.append(x)
    print("\nTHE ENCRYPTED MESSAGE IS")
    for i in r:
        print(i, end=" ")


def decryption(n, private_key, citext):
    print(citext)
    ctext = [int(x) for x in citext.split(',')]
    print(ctext)
    x = []
    for num in ctext:
        x.append(hex(modulus_power(num, private_key, n))[2:].upper())
    r = []
    for word in x:
        r.append(bytes.fromhex(word).decode('utf-8'))
    print(r)
    print("\nTHE DECRYPTED MESSAGE IS")
    for i in r:
        print(i, end="")


def generate_signature(n, k, name):
    r = []
    chunks = [name[i:i + 3] for i in range(0, len(name), 3)]
    print(chunks)
    num = []
    for word in chunks:
        num.append(int(word.encode('utf-8').hex(), 16))
    for text_num in num:
        x = modulus_power(text_num, k, n)
        r.append(x)
    print("\nYOUR DIGITAL SIGNATURE IS")
    for i in r:
        print(i, end=" ")


def signature_verification(n, puk, signature, sender):
    x = []
    for num in [int(x) for x in signature.split(',')]:
        x.append(bytes.fromhex(hex(modulus_power(num, puk, n))[2:].upper()).decode('utf-8'))
    if "".join(x) == sender:
        print("\nTHE SIGNATURE IS VALID.")


def random_select_prime(num=None):
    while True:
        rn = random.randint(32768, 65535)
        if isprime(rn) and rn != num:
            return rn


choice = input("Do you have your p and q?(y or n)")
p = 0
q = 0
if choice == 'y':
    p = int(input("Enter p:"))
    q = int(input("Enter q:"))
else:
    p = random_select_prime()
    print("p:", p)
    q = random_select_prime(p)
    print("q:", q)
n = p * q
print("\nValue of N is:", n)
phin = (p - 1) * (q - 1)
print("φ(N) is:", phin)
e_choice = input("\nDo you have your e?(y or n)")
if e_choice == 'n':
    find_e(phin)
ypuk = int(input("\nType your selected e: "))
print("\nPublic-key(e, N)=({}, {})".format(ypuk, n))
prk = find_d(phin, ypuk)
print("\nENCRYPTION PROCESS")
pn = int(input("\nEnter your Partner's N here:"))
puk = int(input("Enter your Partner's e here:"))
message = input("Enter your text to encrypt here:")
encryption(pn, puk, message)
print("\nDECRYPTION PROCESS")
msg = input("\nEnter the Encrypted Text here:").strip("[]").replace(", ", ",")
decryption(n, prk, msg)
print("\nSIGNING PROCESS")
name = input("\nEnter your name here:")
generate_signature(n, prk, name)
print("\nSIGNATURE VERIFICATION PROCESS")
p_signature = input("\nEnter your partner's signature here:").strip("[]").replace(", ", ",")
pname = input("Enter your partner's name here:")
signature_verification(pn, puk, p_signature, pname)
