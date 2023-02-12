# SRP - Lab 6

## Online and Offline Password Guessing

U ovoj vježbi odradili smo 2 vrste napada: **online** i **offline pogađanje lozinke**. Kod offline pogađanja, napadač nije u mrežnoj interakciji s poslužiteljem dok se kod online sva pogađanja šalju na legitimni server.

### Online password guessing

![https://user-images.githubusercontent.com/92427754/213802498-576dd1b8-4706-42fb-adb6-266bd6811e6b.png](https://user-images.githubusercontent.com/92427754/213802498-576dd1b8-4706-42fb-adb6-266bd6811e6b.png)

Dobili smo podatke o lozinci: sastoji se od malih slova i njena duljina je između 4 i 6 znakova. Koristili smo naredbu ssh kako bismo se sigurno povezali i upravljali poslužiteljem.

`ssh medvidovic_iva@medvidoviciva.local`

*Hydra* je alat za probijanje lozinki koji se može koristiti za izvođenje automatiziranih brute force napada na razne protokole i usluge.  Za daljnji napad nam je bio potreban rječnik. Koristili smo onaj koji je generirao profesor. Dohvatili smo ga pomoću `wget -r -nH -np --reject "index.html*" <http://challenges.local/dictionary/g2/`>. U konačnici smo kombinirali *hydru* i rječnik. U terminalu su se ispisivale razne informacije i u jednom trenu bi ispisivanje prestalo. Tada bi došlo do pogotka lozinke, te smo se uz pomoć nje uspješno prijavili.

### Offline password guessing

Ovjde smo izabrali nečiji profil te smo se pokušali prijaviti kao ta osoba. Naravno da nam je trebala lozinka pa smo je išli otkriti offline putem. Hash vrijednosti dobivene u prethodnom zadatku spremili smo u datoteku. Ponovno smo koristili rječnik (`wget -r -nH -np --reject "index.html*" <http://challenges.local/dictionary/g2/`>) te uspoređivali hash vrijednosti. Ako se pronađe podudaranje, uspješno je probijena lozinka. Koristili smo *hashcat* ovaj put, a o lozinci smo znali da se sastoji od malih slova i ima točno 6 znakova.