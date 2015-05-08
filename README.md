# Συχνά Στοιχειοσύνολα

Σε αρκετές εφαρμογές (μελέτη συμπεριφοράς καταναλωτών, ανάλυση βιοδεικτών, εντοπισμός λογοκλοπής, κ.λπ.) χρειάζεται να βρούμε συχνά στοιχειοσύνολα. Ένας τρόπος εύρεσής τους είναι με τον αλγόριθμο των Agrawal και Srikant. Στη συγκεκριμένη εργασία, καλείστε να υλοποιήσετε τον αλγόριθμο αυτό.

Τα καλάθια αντικειμένων, με τα οποία θα εργαστείτε, θα περιέχονται μέσα σε ένα αρχείο `CSV`. Κάθε γραμμή του αρχείου αυτού θα έχει τη μορφή:
```
item_1, item_2, ..., item_n
```
Για παράδειγμα, ένα τέτοιο αρχείο μπορεί να είναι το παρακάτω:
```
Cat, and, dog, bites
Yahoo, news, claims, a, cat, mated, with, a, dog, and, produced, viable, offspring
Cat, killer, likely, is, a, big, dog
Professional, free, advice, on, dog, training, puppy, training
Cat, and, kitten, training, and, behavior
Dog, &, Cat, provides, dog, training, in Eugene, Oregon
"Dog, and, cat", is, a, slang, term, used, by, police, officers, for, a, male-female, relationship
Shop, for, your, show, dog, grooming, and, pet, supplies
```

Το πρόγραμμά σας θα πρέπει να διαβάζει ένα τέτοιο αρχείο, να βρίσκει συχνά στοιχειοσύνολα σύμφωνα με το κατώφλι που θα δίνει ο χρήστης, και να εξάγει αποτελέσματα σε μορφή `CSV`.

## Απαιτήσεις Προγράμματος

Κάθε φοιτητής θα εργαστεί στο προσωπικό του αποθετήριο στο GitHub. Για να αξιολογηθεί μια εργασία θα πρέπει να πληροί τις παρακάτω προϋποθέσεις:

1. Όλη η εργασία θα πρέπει να βρίσκεται σε έναν κατάλογο `assignment-2015-3` μέσα στο αποθετήριο του φοιτητή.

2. Το πρόγραμμα θα πρέπει να έχει όνομα `a_priori.py`.

3. Το πρόγραμμα θα μπορεί να καλείται ως εξής:
```
python a_priori.py [-n] [-p] [-o OUTPUT] support filename
```
  * Η παράμετρος `-n` είναι προαιρετική. Αν δίνεται η παράμετρος`-n`, το πρόγραμμα θα θεωρεί ότι τα αντικείμενα είναι αριθμητικά (όχι strings). Αλλιώς θα τα θεωρεί strings.
  * Η παράμετρος `-p` είναι προαιρετική. Αν δίνεται η παράμετρος `-p`, το πρόγραμμα θα θεωρεί ότι η ελάχιστη τιμή υποστήριξης που δίνεται μέσω της παραμέτρου `-s` είναι το *ποσοστό* των καλαθιών στα οποία θα πρέπει να βρίσκεται ένα συνολοστοιχείο για να θεωρηθεί σημαντικό.
  * Η παράμετρος `-o OUTPUT` είναι προαιρετική. Αν δίνεται η παράμετρος `-o` το πρόγραμμα θα σώζει τα αποτελέσματα στο αρχείο `OUTPUT`. Αλλιώς θα τα εμφανίζει στην οθόνη. 
  * Η παράμετρος `support` είναι υποχρεωτική. Με αυτήν δίνεται η ελάχιστη τιμή υποστήριξης (support) που θα χρησιμοποιεί ο αλγόριθμος για τον χαρακτηρισμό των συχνών συνολοστοιχείων.
  * Η παράμετρος `filename` είναι υποχρεωτική. Με αυτήν δίνεται το όνομα του αρχείου εισόδου του προγράμματος.
 
Για παράδειγμα, μπορούμε να καλέσουμε το πρόγραμμα ως εξής:

```
python a_priory.p -n 2 a_priori_example_2.csv
```
το οποίο σημαίνει ότι θα διαβαστεί το αρχείο `a_priori_example_2.csv`, τα περιεχόμενα του οποίου θα ερμηνευτούν ως αριθμητικά, η υποστήριξη θα έχει κατώφλι το 2, και τα αποτελέσματα θα εμφανιστούν στην οθόνη.

Αν δώσουμε:

```
python a_priory.p -p -o a_prior_example_1_results.csv 20 a_priori_example_1.csv
```

θα διαβαστεί το αρχείο `a_priori_example_1.csv`, τα περιεχόμενα του οποίου θα ερμηνευτούν ως string, η υποστήριξη θα έχει κατώφλι 20% του συνόλου των εγγραφών, και τα αποτελέσματα θα αποθηκευτούν στο αρχείο `a_prior_example_1_results.csv`.

Το πρόγραμμα θα παράγει το αποτέλεσμα με μορφή `CSV`, όπου ο διαχωριστής θα είναι ο χαρακτήρας `;`. Κάθε γραμμή θα είναι της μορφής:
```
itemset_1:support;itemset_2:support;...itemset_n:support
```
όπου κάθε `itemset` θα είναι της μορφής:

* `(item,)` (αν αποτελείται από ένα αντικείμενο)
* `(item_1, item_2, ..., item_n)` αν αποτελείται από `n` αντικείμενα.

*Προσοχή*: ο λόγος που ζητείται το κάθε itemset να είναι της μορφής `(item,)` αν πρόκειται για μόνο ένα αντικείμενο είναι για τη διευκόλυνσή σας. Είναι πιθανό ότι για την καταμέτρηση των συχνοτήτων των itemsets θα χρησιμοποιήσετε λεξικά. Στα λεξικά τα κλειδιά δεν μπορεί να είναι λίστες ή σύνολα, μπορεί να είναι όπως tuples. Ένα tuple με ένα μόνο στοιχείο item μέσα του αναπαρίσταται στην Python ως `(item,)`.

Έτσι αν στο αρχείο που δώσαμε παραπάνω ως παράδειγμα ζητούσαμε στήριξη 3, θα παίρναμε στην έξοδο:
```
('a',):3;('and',):4;('cat',):5;('dog',):6;('training',):3
('and', 'cat'):3;('and', 'dog'):3;('cat', 'dog'):4
```

Δείγματα αρχείων εισόδου και εξόδου του προγράμματος:

* [example_1.csv](example_1.csv), με στήριξη 3 θα δώσει έξοδο:
```
('a',):3;('and',):4;('cat',):5;('dog',):6;('training',):3
('and', 'cat'):3;('and', 'dog'):3;('cat', 'dog'):4
```
* [example_2.csv](example_2.csv), με στήριξη 2 και θεωρώντας τα αντικείμενα αριθμητικά θα δώσει έξοδο:
```
(1,):2;(2,):3;(3,):3;(5,):3
(1, 3):2;(2, 3):2;(2, 5):3;(3, 5):2
(2, 3, 5):2
```
* [example_3.csv](example_3.csv), με στήριξη 2 και θεωρώντας τα αντικείμενα αριθμητικά θα δώσει έξοδο:
```
(1,):3;(2,):6;(3,):4;(4,):5
(1, 2):3;(1, 4):2;(2, 3):3;(2, 4):4;(3, 4):3
(1, 2, 4):2;(2, 3, 4):2
```
