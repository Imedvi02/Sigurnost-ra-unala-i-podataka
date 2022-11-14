# SRP - LAB 3

```python
from os import path
import base64

from cryptography.hazmat.primitives import hashes
from crypotography.fernet import fernet

def hash(input):
    if not isinstance(input, bytes):
        input = input.encode()

    digest = hashes.Hash(hashes.SHA256())
    digest.update(input)
    hash = digest.finalize()

    return hash.hex()

def test_png(header):
    if header.startswith(b"\211PNG\r\n\032\n"):
        return True
    return False

def brute_force(cyphertext):
    ctr = 0
    while True:
        key_bytes = ctr.to_bytes(32, "big")
        key = base64.urlsafe_b64encode(key_bytes)

        # Now initialize the Fernet system with the given key
        # and try to decrypt your challenge.
        # Think, how do you know that the key tested is the correct key
        # (i.e., how do you break out of this infinite loop)?

        try:
            plaintext = Fernet(key).decrypt(cyphertext)
            header = plaintext[:32]
            if test_png(header):
                print(f"Key: [key]")
                with open("BINGO.png", "wb") as file:
                    file.write(plaintext)
                break
        except Exception:
            pass
        ctr += 1
        if not ctr % 1000:
            print(f"[*] Keys tested: {ctr:,}", end="\r")

if __name__ == '__main__':
    filename = hash('medvidovic_iva') + ".encrypted"

    if not path.exists(filename):
        with open(filename, "wb") as file:
            file.write(b"")
    with open(filename, "rb") as file:
        cyphertext = file.read()

brute_force(cyphertext)
```

Zadatak trećih laboratorijskih vježbi bio je dešifrirati naš osobni crypto izazov, tj. dekriptirati sliku u PNG formatu korištenjem python programskog jezika. Prvo je trebalo pronaći datoteku sa našim izazovom odnosno enkriptirati naziv datoteke. Idući izazov je pronaći odgovarajući ključ, a to se radi Brute-force napadom(metoda pokušaja i pogreške). Korišteni ključ je ograničene entropije - 22 bita što znači da postoji 2^22(nešto više od 4 000 000) kombinacija koje vrtimo pomoću counter-a. Lokalno spremljenu datoteku sa našim izazovom dekriptiramo pomoću high-level sustava za simetričnu enkripciju iz Fernet biblioteke te to spremamo kao plaintext. Zatim trebamo provjeriti je li enkripcija uspjela, tj. ima li naš rezultat smisla. Pošto znamo da se radi o PNG slici, provjerit ćemo da li prva 32 bita plaintexta(koja čine header) počinju sa heksadekadskim vrijednostima 89 50 4E 47 0D 0A 1A 0A. Ako je sve uspješno prošlo plaintext će se spremiti u datoteku PNG formata odnosno sadržaj te datoteke bit će izvorna slika.