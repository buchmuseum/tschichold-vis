# Tschichold-Daten aus dem aktuellen Gesamtabzug extrahieren
pica filter -s "017C.a == 'd030'" DNBGNDtitel.dat.gz -o tschichold.dat

# Auftraggeber
pica filter "028C.4 == 'pat' || 029F.4 == 'pat' || 029F.4 == 'isb'" tschichold.dat | pica select "003@.0, 021A.a, 028C{9,a}, 029F{9,a}" -H "idn,title,auftraggeber-per-idn,auftraggeber-per-name, auftraggeber-inst-idn, auftraggeber-inst-name" -o auftraggeber-neu.csv

# Entstehungsorte

044P $b Entstehungsort
044P $9 040352064 $7 Tg1 $V gik $A gnd $0 4035206-7 $a Leipzig

pica filter "044P/*.b == 'Entstehungsort'" tschichold.dat | pica select "003@.0, 021A.a, 044P/*{V == 'gik',9,a}" -H "idn,title, ort-idn, ort-name" -o entstehungsorte.csv

# Objektgattung

pica filter "044P/01.b == 'Objektgattung'" tschichold.dat | pica select "003@.0, 021A.a, 044P/01{7 == 'Ts1',9,a}" -H "idn, title, ts-idn, ts" -o objektgattungen.csv

# Druckschriften

pica filter "022A.a =~ 'Druckschrift'" tschichold.dat | pica select "003@.0, 021A.a, 022A.a" -H "idn, title, schrift" -o druckschriften.csv