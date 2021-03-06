# Joins

Alle Beispiele verwenden die im selben Verzeichnis vorhandene Beispieldatenbank "futter.sql"

## Cross join
Alle Datensätze beider Tabellen werden miteinander verknüpft. Die Länge der Ergebnistabelle ist das Produkt aus den längen der beiden Tabellen. Das Schlüsselwort "CROSS" kann dabei entfallen.
### Beispiel
* Alle Speisen und alle Beilagen sollen in einer Tabelle (Speisekarte) angezeigt werden.
```SQL
SELECT * FROM `hauptgerichte` CROSS JOIN `beilagen`
bzw
SELECT h.`bezeichnung` AS 'Hauptgericht',
    b.`bezeichnung` AS 'Beilage',
    h.`preis` + b.`preis` AS 'Preis'
    FROM `hauptgerichte` AS h CROSS JOIN `beilagen` AS b
```
* Verschiedene miteinander kombinierbare Versicherungen (z. B. Haftpflicht und Unfall)

## Inner join
Alle Datensätze beider Tabellen werden miteinander verknüpft. Die Länge der Ergebnistabelle ist das Produkt aus den längen der beiden Tabellen. Hier ist es möglich durch Vergleich zweier Schlüssel die Auswahl zu beschränken (ON).
Ist die Bezeichnung beider Schlüssel in beiden Tabellen identisch kann der Vergleich durch USING(key) durchgeführt werden. Fehlt eine Beschränkung (ON/USING) wird aus einem INNER JOIN automatisch ein CROSS JOIN.
Das Schlüsselwort "INNER" kann dabei entfallen.
### Beispiel
* Anzeige aller Kundenreservierungen
```SQL
SELECT * FROM `kunden` AS k INNER JOIN `reservierungen` AS r
    ON k.`idKunde` = r.`idKunde`
bzw
SELECT * FROM `kunden` INNER JOIN `reservierungen`
    USING(`idKunde`)
bzw
SELECT k.`name` AS 'Kunde',
    r.`datum` AS 'Zeitpunkt'
    FROM `kunden` AS k INNER JOIN `reservierungen` AS r
    ON k.`idKunde` = r.`idKunde`
```

## Natural join
Alle Datensätze beider Tabellen werden miteinander verknüpft. Es werden nur die Einträge in die Ergebnistalle übernommen, die mindestens einen gemeinsamen Schlüssel haben. Hierbei handelt es sich um einen INNER JOIN, der automatisch nach passenden PS/FS-Kombinationen sucht.
### Beispiel
* Anzeige aller Kundenreservierungen
```SQL
SELECT * FROM `kunden` NATURAL JOIN `reservierungen`
bzw
SELECT
    k.`name` AS 'Kunde',
    r.`datum` AS 'Zeitpunkt'
    FROM `kunden` AS k NATURAL JOIN `reservierungen` AS r
```

## Left outer join
Alle Datensätze beider Tabellen werden miteinander verknüpft. Es werden alle Einträge der linken Tabelle in die Ergebnistalle übernommen, evtl. fehlende Einträge der rechten Tabelle werden durch NULL aufgefüllt. Das Schlüsselwort "OUTER" kann dabei entfallen.
### Beispiel
* Anzeige aller Kunden und evtl. Kundenreservierungen
```SQL
SELECT * FROM `kunden` AS k
    LEFT OUTER JOIN `reservierungen` AS r
    ON k.`idKunde` = r.`idKunde`
bzw
SELECT * FROM `kunden`
    LEFT OUTER JOIN `reservierungen`
    USING(`idKunde`)
bzw
SELECT
    k.`name` AS 'Kunde',
    r.`datum` AS 'Zeitpunkt'
    FROM `kunden` AS k
    LEFT OUTER JOIN `reservierungen` AS r
    USING(`idKunde`)
```

## Right outer join
Alle Datensätze beider Tabellen werden miteinander verknüpft. Es werden alle Einträge der rechten Tabelle in die Ergebnistalle übernommen, evtl. fehlende Einträge der linken Tabelle werden durch NULL aufgefüllt. Das Schlüsselwort "OUTER" kann dabei entfallen.
### Left und Right join
Beide unterscheiden sich nur in der Reihenfolge der Tabellen
```SQL
SELECT * FROM a LEFT JOIN b
ist identisch mit
SELECT * FROM b RIGHT JOIN a
```

## Full join
Alle Datensätze beider Tabellen werden miteinander verknüpft. Es werden alle Einträge beider Tabellen in die Ergebnistalle übernommen, evtl. fehlende Einträge werden durch NULL aufgefüllt. Achtung: MySQL/MariaDB unterstützt keinen FULL OUTER JOIN.
