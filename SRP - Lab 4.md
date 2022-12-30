# SRP - Lab 4

```jsx
from cryptography.hazmat.primitives import hashes, hmac
```

```jsx
from cryptography.hazmat.primitives import hashes, hmac
```

```jsx
from cryptography.exceptions import InvalidSignature
```

```jsx
from pathlib import Path
```

```jsx
import re
```

```jsx
import datetime
```

```jsx
def verify_MAC(key, signature, message):
```

```jsx
if not isinstance(message, bytes):
```

```jsx
message = message.encode()
```

```jsx
h = hmac.HMAC(key, hashes.SHA256())
```

```jsx
h.update(message)
```

```jsx
try:
```

```jsx
h.verify(signature)
```

```jsx
except InvalidSignature:
```

```jsx
return False
```

```jsx
else:
```

```jsx
return True
```

```jsx
def generate_MAC(key, message):
```

```jsx
if not isinstance(message, bytes):
```

```jsx
message = message.encode()
```

```jsx
h = hmac.HMAC(key, hashes.SHA256())
```

```jsx
h.update(message)
```

```jsx
signature = h.finalize()
```

```jsx
return signature
```

```jsx
if __name__ == "__main__":
```

```jsx
# 1. Sign the message
```

```jsx
# 1.1. Read the file content
```

```jsx
# Reading from a file
```

```jsx
#with open(f"message.txt", "rb") as file:
```

```jsx
# message = file.read()
```

```jsx
#print(message)
```

```jsx
# 1.2. Generate signing key/secret
```

```jsx
#key = "my super secret".encode()
```

```jsx
# 1.3. Actually sign the message
```

```jsx
#signature = generate_MAC(key=key, message=message)
```

```jsx
#print(signature)
```

```jsx
# 1.4. Save the signature/MAC/auth_tag into a separate file
```

```jsx
#with open("message.sig", "wb") as file:
```

```jsx
# file.write(signature)
```

```jsx
# 2. Verify message authencity
```

```jsx
# 2.1. Read the message file content and the signature
```

```jsx
#with open("message.txt", "rb") as file:
```

```jsx
# message = file.read()
```

```jsx
#with open("message.sig", "rb") as file:
```

```jsx
# signature = file.read()
```

```jsx
# 2.2. Get/learn the signing key
```

```jsx
#key = "my super secret".encode()
```

```jsx
# 2.3. Sign the message and compare locally generated MAC with the received one
```

```jsx
#is_authentic = verify_MAC(key=key, signature=signature, message=message)
```

```jsx
#print(f"Message is {'OK' if is_authentic else 'NOK'}")
```

```jsx
#2. zadatak
```

```jsx
PATH = "challenges/g3/medvidovic_iva/mac_challenge/"
```

```jsx
KEY = "medvidovic_iva".encode()
```

```jsx
messages = []
```

```jsx
for ctr in range(1, 11):
```

```jsx
msg_filename = f"order_{ctr}.txt"
```

```jsx
sig_filename = f"order_{ctr}.sig"
```

```jsx
msg_file_path = Path(PATH + msg_filename)
```

```jsx
sig_file_path = Path(PATH + sig_filename)
```

```jsx
with open(msg_file_path, "rb") as file:
```

```jsx
message = file.read()
```

```jsx
with open(sig_file_path, "rb") as file:
```

```jsx
signature = file.read()
```

```jsx
is_authentic = verify_MAC(key=KEY, signature=signature, message=message)
```

```jsx
#print(f'Message {message.decode():>45} {"OK" if is_authentic else "NOK":<6}')
```

```jsx
if is_authentic:
```

```jsx
messages.append(message.decode())
```

```jsx
messages.sort(key=lambda m: datetime.datetime.fromisoformat(re.findall(r'
```

```jsx
.∗?
```

```jsx
.∗?
```

```jsx
', m)[0][1:-1]))
```

```jsx
for m in messages:
```

```jsx
print(f"Message {m:>45} OK")
```

### Zadatak 1.

Korištenjem MAC algoritma  bilo je potrebno implementirati zaštitu integriteta sadržaja tekstualne datoteke. Prvo smo kreirali datoteku čiji integritet želimo zaštiti u folderu u kojem radimo pa smo njen sadržaj učitali u memoriju. Za zaštitu su nam bile potrebne dvije funkcije: za izračun MAc vrijednosti poruke koja se šalje i za provjeru validnosti MAC-a. Nakon čitanja sadržaja datoteke i generiranja MAC vrijednosti uz pomoć ključa “potpis” se šalje posebnom datotekom. Kada primatelj primi datoteku i potpis, mora lokalno generirati  MAC vrijednost, odnosno potpis i usporediti ga sa primljenim.

### Zadatak 2.

Korištenjem MAC algoritma bilo je potrenbno utvrditi vremenski autentičnu sekvencu transakcija dionica. Svatko je preuzeo personalizirani izazov. Ključ u MAC algoritmu smo dobili iz našeg imena. Nakon provjere autentičnosti svih poruka na isti način kao u prvom zadatku sortirali smo ih u odgovarajući niz.