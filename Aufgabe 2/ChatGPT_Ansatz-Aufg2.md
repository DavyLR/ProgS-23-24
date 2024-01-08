# Aufgabe 1

- Die Regel einBegleiter/2 kann wie folgt definiert werden:

```
einBegleiter(Gott, Begleiter) :- begleiter(Begleiter, Gott).
```

- Die Regel alleBegleiterEinesWanen/1 kann wie folgt definiert werden:

```
alleBegleiterEinesWanen(Begleiter) :- wane(Gott), begleiter(Begleiter, Gott).
```

- Die Regel mehrfach/1 kann wie folgt definiert werden:

```
mehrfach(Begleiter) :- begleiter(Begleiter, Gott1), begleiter(Begleiter, Gott2), not(Gott1 = Gott2).
```

- Das 4-Port-Modell für die Regel example/1 sieht wie folgt aus:

```
Call: example(G)
    Call: begleiter(N, loki)
        Exit: begleiter(hildsvini, freyja)
    Exit: begleiter(hildsvini, freyja)
    Call: begleiter(hildsvini, G)
        Exit: begleiter(hildsvini, freyja)
    Exit: example(freyja)
```

Der Cut-Operator ! in der Regel example/1 sorgt dafür, dass Prolog keine weiteren Lösungen für begleiter(N, G) sucht, sobald eine Lösung gefunden wurde. Das bedeutet, dass der zweite Aufruf von begleiter(N, G) nur einmal ausgeführt wird, auch wenn es mehrere Lösungen gibt. In diesem Fall wird der Begleiter hildsvini nur einmal zurückgegeben, obwohl er sowohl Freyja als auch Loki begleitet.

# Aufgabe 2

- Die Regel all_members/2 kann wie folgt definiert werden:

```
all_members([], _).
all_members([H|T], List) :- member(H, List), all_members(T, List).
```

- Die Regel remove_second_last/2 kann wie folgt definiert werden:

```
remove_second_last([_], []) :- !.
remove_second_last([_, _ | T], [H | R]) :- remove_second_last([_|T], [H | R]).
```

- Die Regel double_odd/2 kann wie folgt definiert werden:

```
double_odd([], []).
double_odd([H|T], [H2|R]) :- 1 is H mod 2, H2 is H * 2, double_odd(T, R).
double_odd([H|T], [H|R]) :- 0 is H mod 2, double_odd(T, R).
```

- Die Regel sum_neighbours/2 kann wie folgt definiert werden:

```
sum_neighbours([_], false) :- !.
sum_neighbours([A, B], [A+B]) :- !.
sum_neighbours([A, B | T], [A+B | R]) :- sum_neighbours([B | T], R).
sum_neighbours([A, B, C | T], R) :- sum_neighbours([B, C | T], R).
sum_neighbours([A, B, C, D | T], R) :- sum_neighbours([C, D | T], R).
```

# Aufgabe 3

## 1.

(a) Die Fakten können wie folgt definiert werden:

aussage(siegfried, Ritter, _, _).
aussage(roderich, _, _, Spion).
aussage(albert, _, Knappe, _).

(b) Die Regel `rollenverteilung/3` kann wie folgt definiert werden:
```
rollenverteilung(Ritter, Knappe, Spion) :-
    permutation([siegfried, albert, roderich], [Ritter, Knappe, Spion]),
    aussage(siegfried, Ritter, _, _),
    aussage(roderich, _, _, Spion),
    aussage(albert, _, Knappe, _),
    not(Ritter = Knappe),
    not(Ritter = Spion),
    not(Knappe = Spion).
```

Die Lösungen für die Rollenverteilung sind:
```
Ritter = albert, Knappe = siegfried, Spion = roderich ;
Ritter = albert, Knappe = roderich, Spion = siegfried.
```
Also, Albert ist der Ritter, Siegfried ist der Knappe und Roderich ist der Spion.

(c)
```
loeseRaetsel(Ritter, Knappe, Spion) :-
    rollenverteilung(Ritter, Knappe, Spion),
    aussage(siegfried, Ritter, _, _),
    aussage(roderich, _, _, Spion),
    aussage(albert, _, Knappe, _),
    not(aussage(Knappe, _, Knappe, _)),
    not(aussage(Spion, _, _, Spion)).
```
