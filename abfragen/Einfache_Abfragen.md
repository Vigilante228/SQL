# Einfache Abfragen
Manchmal sind mehrere Lösungen möglich
1. Suchen Sie alle Nahrungsmittel, die mehr als 5 € kosten.
```SQL
SELECT *
    FROM `hauptgerichte`
    WHERE `preis` > 5

Oder alle Nahrungsmittel (Hauptgericht und Beilage):
SELECT *
    FROM `hauptgerichte`
    WHERE `preis` > 5
UNION
SELECT *
    FROM `beilagen`
    WHERE `preis` > 5
```

2. Suchen Sie alle Nahrungsmittel, die zwischen 5 € und 7 € (inklusive) kosten.
```SQL
SELECT *
    FROM `hauptgerichte`
    WHERE `preis` >= 5 AND `preis` <= 7
bzw
SELECT *
    FROM `hauptgerichte`
    WHERE `preis` BETWEEN 5 AND 7
```

3. Erstellen Sie eine Speisekarte mit den folgenden Spalten: Hauptgericht, Beilage, Preis
```SQL
SELECT
    h.`bezeichnung` AS 'Hauptgericht',
    b.`bezeichnung` AS 'Beilage',
    h.`preis` * b.`preis` AS 'Preis'
    FROM `hauptgerichte` h
    CROSS JOIN `beilagen` b
```

4. Listen Sie alle Wiesbadener Kunden auf
```SQL
SELECT
    *
    FROM `kunden`
    WHERE `wohnort` = 'Wiesbaden'
```

5. Wie viele Kunden leben in Niedernhausen, das Ergebnis soll wie folgt aussehen: "{ANZAHL} Kunden wohnen in Niedernhausen"
```SQL
SELECT
    CONCAT(
        COUNT(`idKunde`),
        ' Kunden wohnen in Niedernhausen'
    ) AS 'Ergebnis'
    FROM `kunden`
    WHERE `wohnort` = 'Niedernhausen'
```

6. Listen Sie alle Kunden auf, die bereits eine Reservierung gemacht haben
```SQL
SELECT
    DISTINCT `name`
    FROM `kunden`
    NATURAL JOIN `reservierungen`
```

7. Listen Sie alle Kunden auf und fügen Sie eventuelle Reservierungen hinzu. Das Ergebnis soll nur den Namen und das Datum beinhalten
```SQL
SELECT
    `name` AS 'Kunde', `datum` AS 'Datum'
    FROM `kunden`
    LEFT OUTER JOIN `reservierungen`
    USING(`idKunde`)
```

8. Aus wie vielen Städten stammen die Kunden
```SQL
SELECT
    COUNT(DISTINCT `wohnort`)
    FROM `kunden`
```

9. Nennen Sie die Städte (jeweils nur einmal)
```SQL
SELECT
    DISTINCT `wohnort`
    FROM `kunden`
```

10. Listen Sie alle Kunden auf, sortiert nach Nach- und Vorname
```SQL
SELECT
    *
    FROM `kunden`
    ORDER BY `nachname`, `vorname`
```

11. Finden Sie alle Kunden, die 'Müller' heißen und bereits mindestens einmal Reserviert haben
```SQL
SELECT * FROM `kunden`
	NATURAL JOIN `reservierungen`
    WHERE `nachname` = 'Müller'
```

12. Beim welchem Lieferanten kann 'Eisbergsalat' bestellt werden
```SQL
SELECT
	*
    FROM `lieferanten`
    NATURAL JOIN `beilagelieferant`
    NATURAL JOIN `beilagen` AS b
    WHERE b.`bezeichnung` = 'Eisbergsalat'
```

13. Wie viele \'Wassermann\'s sind in der Datenbank eingetragen
```SQL
SELECT
    COUNT(*)
    FROM `kunden`
    WHERE `nachname` = 'Wassermann'
```
14. Wie viele \`Nachname\`n sind in der Datenbank eingetragen
```SQL
SELECT COUNT(DISTINCT `nachname`) AS 'Anzahl'
	FROM `kunden`
```

15. Zeigen Sie alle Nahrungsmittel mit allen `Lieferanten`
```SQL
SELECT `bezeichnung`, `firma` FROM `lieferanten`
	NATURAL JOIN `hauptgerichtlieferant`
    NATURAL JOIN `hauptgerichte`

UNION

SELECT `bezeichnung`, `firma` FROM `lieferanten`
	NATURAL JOIN `beilagelieferant`
    NATURAL JOIN `beilagen`
```

16. Zeigen Sie alle Kunden und ihre bestellten Hauptgerichte. Das Ergebnis soll wie folgt angezeigt werden:
Kunde (enthält \`vorname\` und \`nachname\`, 'Datum', 'Menge', 'Bezeichnung', 'Preis')
```SQL
SELECT
	CONCAT(`vorname`, ' ', `nachname`) AS 'Kunde',
    `datum` AS 'Datum',
    `menge` AS 'Anzahl',
    `bezeichnung` AS 'Bezeichnung',
    `menge` * `preis` AS 'Preis'
    FROM `kunden`
    NATURAL JOIN `bestellungen`
    NATURAL JOIN `bestellunghauptgericht`
    NATURAL JOIN `hauptgerichte`
```

17. Ändern Sie die Stadt von 'Susi Sorglos' in 'Wiesbaden'
```SQL
UPDATE `kunden`
    SET `wohnort` = 'Wiesbaden'
    WHERE `vorname` = 'Susi' AND `nachname` = 'Sorglos'
```

18. Löschen Sie den Eintrag von 'Susi Sorglos'
```SQL
DELETE FROM `kunden`
    WHERE `vorname` = 'Susi' AND `nachname` = 'Sorglos'
```
19. Warum schlögt diese Anweisung fehl

# Abfragen mit Gruppierungen
1. Listen Sie die Städte der Kunden und die Anzahl der Kunden aus der jeweiligen Stadt, zeigen Sie die Stadt mit den meisten Kunden zuerst
```SQL
SELECT
    `wohnort` AS 'Wohnort',
    COUNT(`idKunde`) AS 'Kunden'
    FROM `kunden`
    GROUP BY `wohnort`
    ORDER BY COUNT(`idKunde`) DESC
```

2. Wie oft kommt jeder \`Nachname\` vor
```SQL
SELECT `nachname`, COUNT(*)
	FROM `kunden`
	GROUP BY `nachname`
```

3. Wie viele Hauptgerichte werden von jedem Lieferanten geliefert
```SQL
SELECT
    `firma` AS 'Firma',
    COUNT(*) AS 'Anzahl'
    FROM `lieferanten`
    NATURAL JOIN `hauptgerichtlieferant`
    NATURAL JOIN `hauptgerichte`
    GROUP BY `firma`
```

4. Wie viele Nahrungsmittel werden von jedem Lieferanten geliefert
```SQL
SELECT
	`firma` AS 'Firma',
    COUNT(*) AS 'Anzahl'
    FROM
    (
        SELECT
            *
            FROM `lieferanten`
            NATURAL JOIN `hauptgerichtlieferant`
            NATURAL JOIN `hauptgerichte`
         UNION
         SELECT
            *
            FROM `lieferanten`
            NATURAL JOIN `beilagelieferant`
            NATURAL JOIN `beilagen`
    ) AS a
	GROUP BY `firma`
```
