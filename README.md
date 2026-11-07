# Jedi Datenbank

Node.js, keine Abhängigkeiten. Start: `node server.js` (Port aus `PORT`).

## Render
1. Alle Dateien dieses Ordners in das GitHub-Repo, direkt in die Wurzel (Dockerfile, server.js, package.json, public/ ...).
2. Render -> New -> Web Service -> Repo wählen.
3. Language/Runtime: **Node**, Build Command: leer oder `npm install`, Start Command: `node server.js`.
   (Alternativ Docker: das mitgelieferte Dockerfile wird dann gefunden.)
4. Optional unter Environment: `OWNER_PASSWORD` = dein eigenes Passwort (überschreibt das Standardpasswort).

## Login
Haupt-Owner: Benutzer `owner`. Passwort ändern unter "Konto".
Rollen: Jedi (nur lesen) < Gelehrter (Personen, Berichte, Operationen starten) < Wissensrat (alle Inhalte inkl. Karte, Operationen verwalten) < Jedi Leitung (zusätzlich Konten verwalten).
Jedi-Leitungs-Konten legt nur das Haupt-Owner-Konto an und verwaltet sie auch allein.

## Neue Konten
Beim Anlegen erzeugt die Seite ein Passwort (einmalig sichtbar, mit Kopieren-Knopf). Bei der ersten Anmeldung muss der Nutzer ein eigenes wählen: mindestens 5 Zeichen mit Buchstaben und Zahlen. "Neues Passwort erzeugen" in der Kontenliste setzt ein Konto zurück.

## Dauerhafter Speicher (wichtig)
Render Free hat ein flüchtiges Dateisystem und schläft nach ca. 15 Minuten ohne Besuch ein. Beim nächsten Start sind alle Änderungen weg.
Kostenlose Lösung: Upstash Redis (upstash.com, Konto ohne Kreditkarte).
1. Upstash -> Create Database (Redis), Region EU -> Reiter "REST API".
2. Die Werte "UPSTASH_REDIS_REST_URL" und "UPSTASH_REDIS_REST_TOKEN" in Render unter Environment eintragen.
3. Speichern, Render startet neu. Im Log steht "Dauerspeicher aktiv", unter Konto "Speicher: dauerhaft".
Alternative: bezahlte Persistent Disk und `DATA_DIR` auf deren Pfad setzen.

## Berichte
Berichte, Orders und Dokumente schreibt man selbst (ab Gelehrter). Geheimhaltung: Offen = alle, Intern = ab Gelehrter, Vertraulich und Geheim = ab Wissensrat.
Eigene Berichte bearbeiten/löschen; fremde nur Wissensrat und Jedi Leitung.

## Operationen
Starten ab Gelehrter (Name und Einsatzgebiet frei wählbar). Umbenennen, vorantreiben, abbrechen, archivieren: ab Wissensrat.

## Anpassung (ab Wissensrat)
Unter "Konto" lassen sich Ränge, Labels (Zugehörigkeit: Republik, KUS, Neutral, Dunkle Machtpräsenz, mit Farbe und "Bedrohung"-Markierung) und Berichtsarten ändern. Umbenennen zieht alle bestehenden Einträge mit. Entfernen geht nur, wenn nichts mehr damit verknüpft ist.

## Notfall (UMBRA)
Wissensrat und Jedi Leitung können das Notfallprotokoll auslösen und beenden. Solange es aktiv ist, sind alle Konten unterhalb des Wissensrats gesperrt (Sperrbildschirm, der Server liefert keine Daten). Die Seite öffnet sich bei Ende von selbst wieder.

## Akten bei Personen
Im Personen-Register zeigt jede Person unten ihre Akten (neueste oben). Jede Akte hat Titel und freien Text. Anlegen ab dem Recht „Schreiben“; eigene Akten bearbeiten/löschen, fremde nur mit „Verwalten“.

## Gruppen und Rechte (Jedi Leitung)
Unter „Konto“ → „Gruppen und Rechte“ legt die Jedi Leitung pro Gruppe einzeln fest: Personen, Akten, Berichte (inkl. Anhänge), Operationen starten, Operationen steuern, Karte, Fremde Inhalte ändern, Anpassung, Notfall, Offen lesen, Intern lesen, Vertraulich lesen, Logs. Mit „Offen lesen“ legst du fest, wer Berichte der Stufe „Offen“ sehen darf (bestehende Gruppen behalten es zunächst). Alte Rechte „Schreiben“/„Verwalten“ werden beim ersten Start automatisch auf die passenden Einzelrechte umgestellt. Neue Gruppen lassen sich dort anlegen und danach Konten zuweisen. Grundgruppen bleiben bestehen, die Jedi Leitung hat immer alle Rechte. Eine Gruppe lässt sich nur entfernen, wenn kein Konto sie mehr nutzt.

## Sichtbarkeit nach Gruppen
Personen, Akten (bei Personen) und Operationen haben ein Feld „Sichtbar für“. Standard: alle Gruppen. Wählt man „Alle Gruppen“ ab, sehen nur die angehakten Gruppen den Eintrag; die anderen sehen ihn gar nicht (der Server liefert ihn nicht aus). Die Jedi Leitung sieht immer alles. Bei Operationen ändert man die Sichtbarkeit über den Knopf „Sichtbarkeit“ (ab Verwalten). Wird eine Gruppe gelöscht, bleiben ihre Einträge eingeschränkt (dann nur Jedi Leitung).

## Kontonamen
Namen dürfen Leerzeichen und Großbuchstaben enthalten (z. B. „Nario Zeyscho“). Bei der Anmeldung ist die Schreibweise egal. Beim Namen lässt sich nachträglich nur die Groß-/Kleinschreibung ändern.

## Farben
Labels können Weiß, Hellgrau, Silber, Mittelgrau, Rot, Dunkelrot, Orange, Gelb, Grün, Dunkelgrün, Hellblau, Blau, Dunkelblau, Lila oder Pink sein.

## Anhänge an Berichten
Jeder Bericht hat unten „Anhänge“ (Titel + freier Text, wie die Akten bei Personen), jeweils mit eigener Sichtbarkeit nach Gruppen. Hinzufügen mit dem Recht „Berichte“, fremde Anhänge ändern mit „Fremde Inhalte“.

## Logs
Menüpunkt „Logs“ (Recht „Logs“, standardmäßig nur Jedi Leitung): zeigt, wer wann was gemacht hat (Anmeldungen inkl. Fehlversuche, Personen, Akten, Berichte, Anhänge, Operationen, Karte, Anpassung, Notfall, Rechte, Konten). Gespeichert werden die letzten 3000 Aktionen. Filter: Suche, Konto, Bereich (z. B. Personen, Berichte, Konten), Aktion, Zeitraum (von/bis, Heute, 7 Tage, 30 Tage) und „Nur kritische“ (Fehlversuche, Löschungen, Notfall).

## Sortierung im Personenregister
Standard ist „Rang“: in der Reihenfolge der Ränge unter „Anpassung“ (oben = zuerst, mit ▲ ▼ verschiebbar), innerhalb eines Rangs das neueste zuletzt.

## Personen beschreiben
Jede Akte hat zusätzlich zu Name und Rang die Felder Spezies und Aussehen sowie beliebig viele eigene Felder (Bezeichnung + Beschreibung, bis 20, z. B. Herkunft, Waffen, Fähigkeiten). Sie stehen im Formular „Akte anlegen/bearbeiten“, erscheinen in der Detailansicht, die Spezies ist auch über die Suche findbar. Änderungen daran stehen im Log.

## Zugehörigkeit und Beschattung
Im Personenregister heißt das frühere „Status“ jetzt „Zugehörigkeit“. „Kontakt aufnehmen“ gibt es nicht mehr. Beschattete Personen sind sichtbar: rotes Label „● Beschattet“ und roter Balken in der Liste, Banner in der Detailansicht.

## Rechte „Konten“ und „Gruppen“
Unter „Gruppen und Rechte“ gibt es zwei neue Rechte. „Konten“: Konten anlegen, Gruppe zuweisen, Passwort zurücksetzen, löschen. „Gruppen“: Gruppen und ihre Rechte verwalten. Standardmäßig hat sie nur die Jedi Leitung. Wer sie ohne Jedi Leitung bekommt, kann keine Rechte erhöhen: Es lassen sich nur Gruppen vergeben, deren Rechte man selbst hat, nur eigene Rechte an Gruppen vergeben oder entziehen, und Konten oder Gruppen mit höheren Rechten, das eigene Konto sowie Jedi-Leitungs-Konten bleiben tabu. Jedi-Leitungs-Konten legt weiterhin nur das Haupt-Owner-Konto an.

## Temporäre Konten
Beim Anlegen eines Kontos (Recht „Konten“) lässt sich unter „Konto-Dauer“ eine Befristung wählen (1 Stunde bis 30 Tage oder ein eigenes Datum mit Uhrzeit). Nach Ablauf ist das Konto sofort gesperrt: bestehende Sitzungen enden, die Anmeldung zeigt „Konto abgelaufen“. Das Konto bleibt in der Liste (mit Label „abgelaufen“) und seine Einträge und Logs bleiben erhalten. In der Kontenliste lässt sich die Gültigkeit jederzeit verlängern oder auf „Unbefristet“ setzen, oder das Konto löschen. Jedi-Leitungs-Konten sind nicht befristbar. Das Anlegen, Verlängern und Entfristen steht im Log.

## Ränge für Gruppen
Jede Gruppe hat in der Tabelle „Gruppen und Rechte“ einen Rang von 1 bis 10 (Standard: Jedi 1, Gelehrter 2, Wissensrat 3, neue Gruppen 1). Die Jedi Leitung steht über allen Rängen und ist nicht eingeschränkt. Alle anderen mit den Rechten „Konten“ oder „Gruppen“ dürfen nur Gruppen und Konten mit niedrigerem Rang verwalten: Eine Rang-4-Gruppe kann keine Rang-5-Gruppe bearbeiten, und eine Rang-5-Gruppe auch keine andere Rang-5-Gruppe. Das gilt fürs Anlegen, Umbenennen, Rechte ändern, Rang ändern und Entfernen von Gruppen sowie fürs Anlegen, Zuweisen, Zurücksetzen und Löschen von Konten. Neue oder geänderte Gruppen müssen ebenfalls einen niedrigeren Rang als die eigene haben. Die Regel „nur Rechte vergeben, die man selbst hat“ gilt zusätzlich. Wichtig: Vorhandene eigene Gruppen mit Konten-/Gruppenrecht haben zunächst Rang 1 und können deshalb erst etwas verwalten, wenn die Jedi Leitung ihnen einen höheren Rang gibt.

## Sortierung der Gruppen und Konten sperren
Gruppen stehen überall (Tabelle, Auswahllisten) sortiert: zuerst die Jedi Leitung, dann nach Rang von hoch nach niedrig, bei gleichem Rang alphabetisch. Neu angelegte Gruppen rutschen nach dem Speichern an ihre Stelle.
Konten lassen sich in der Kontenliste mit „Sperren“ deaktivieren (Recht „Konten“, Rangregeln wie oben). Ein gesperrtes Konto ist sofort ausgeloggt und kann sich nicht anmelden („Konto gesperrt“), bleibt aber mit allen Einträgen bestehen. „Entsperren“ hebt es auf. Das eigene Konto und das Haupt-Owner-Konto lassen sich nicht sperren. Sperren und Entsperren stehen im Log.

## Jedi Ermittler
Die Jedi Leitung kann Konten zu „Jedi Ermittler“ machen (beim Anlegen unter „Jedi Ermittler“ oder in der Kontenliste mit „Jedi Ermittler“ / „Ermittler entziehen“; das Haupt-Owner-Konto kann sich selbst freigeben). Nur diese Konten sehen den Reiter „Jedi Ermittler“, auch die Jedi Leitung nicht ohne Freigabe. Der Server liefert die Daten ausschließlich an freigegebene Konten und nie über die normalen Datenabfragen aus. Dort tragen sie ein: Tarnidentitäten (Tarnname, Status, Einsatzort, Legende, Notiz), Informanten (Name, Zuverlässigkeit, Status, Quelle, Kontaktweg, Ort, Notiz) und Gespräche (Betreff, Zeitpunkt, Gesprächspartner, Tarnidentität, Informant, Ort, Protokoll). Alle Ermittler sehen alle Einträge; eigene Einträge bearbeitet und löscht man selbst, fremde nur mit dem Recht „Fremde Inhalte“. Im Log steht nur, dass ein Eintrag angelegt, geändert oder gelöscht wurde, nie der Inhalt. Wird die Freigabe entzogen, ist der Reiter sofort weg; die Einträge bleiben erhalten.

## Name
Die Seite heißt „Jedi Datenbank“ (Titel, Kopf, Anmeldung, Fußzeile).

## Phasen der Operationen und Sortierung der Konten
Operationen durchlaufen feste Phasen: 0 % Planung, 25 % Mitglieder Sammlung, 50 % Briefing, 75 % Durchführung, 100 % Abgeschlossen. Neue Operationen starten bei „Planung“, der Knopf „Weiter: …“ (ab dem Recht „Op. steuern“) setzt die nächste Phase. Bei „Abgeschlossen“ lässt sich die Operation archivieren. Bereits vorhandene Operationen werden beim Start auf die nächstniedrigere Phase gesetzt. Die Phasenwechsel stehen im Log.
Die Kontenliste ist nach Rang sortiert: zuerst unantastbare Konten, dann das Haupt-Owner-Konto, dann nach Rang der Gruppe von hoch nach niedrig, bei gleichem Rang alphabetisch.

## Unantastbare Konten
Unantastbare Konten sind fest im Code (`UNTOUCHABLE` in `server.js`) hinterlegt, aktuell „Mace Windu“ mit der Gruppenbezeichnung „Jedi Ratsvorsitz“ (technisch die Rechte der Jedi Leitung, also alle Rechte; die Gruppe „Jedi Leitung“ selbst heißt weiterhin so). Das Konto steht normal in der Kontenliste unter „Konten“, mit dem Label „Unantastbar“ und ohne Bearbeiten-Knöpfe, immer ganz oben (noch vor dem Haupt-Owner-Konto). Es kann von niemandem geändert werden, auch nicht vom Haupt-Owner: kein Löschen, Sperren, Befristen, Gruppenwechsel, Passwort-Reset und keine Umbenennung (der Server lehnt das ab). Nur das Konto selbst ändert sein Passwort unter „Konto“. Beim ersten Login muss das Startpasswort ersetzt werden. Wird das Konto in den Daten gelöscht, gesperrt oder herabgestuft, stellt der nächste Start es wieder her. Notfall-Zugang bei vergessenem Passwort: In Render die Umgebungsvariable `UNTOUCHABLE_PASSWORD_MACE_WINDU` setzen (dann gilt dieses Passwort ohne erzwungene Änderung). Weitere Konten: in `server.js` zur Liste `UNTOUCHABLE` ergänzen (Passwort-Hash mit `node server.js --hash "passwort"`).

## Archiv
Neuer Reiter „Archiv“ (SR-AR) mit zwei Bereichen, zwischen denen man oben umschaltet; beide haben eine Suche und zeigen die Einträge als Karten.
- **Machtfähigkeiten:** Karte mit Icon oben, darunter Name, darunter die Beschreibung. Das Icon lädt man als Bild hoch (PNG, JPEG, WebP, GIF); die Seite verkleinert es automatisch auf höchstens 128×128 Pixel. „Icon entfernen“ nimmt es wieder weg. Name und Beschreibung sind frei schreibbar.
- **Prüfungen:** Karte mit dem Prüfungsnamen als Kopfzeile, darunter eine Zeile „Prüfungs Leiter“ und darunter die Prüfungsbeschreibung. Die Leiter wählt man aus einer Liste aller Konten und aller Personen aus dem Register (die man sehen darf); mehrere sind möglich, mit × nimmt man einen wieder heraus. Umbenannte Konten oder Personen erscheinen automatisch mit dem neuen Namen.
Lesen dürfen alle Konten. Eintragen, ändern und löschen geht mit dem neuen Gruppenrecht „Archiv“ (unter „Gruppen und Rechte“); eigene Einträge bearbeitet man selbst, fremde nur mit „Fremde Inhalte“. Standardmäßig haben das Recht Gelehrter, Wissensrat und die Jedi Leitung; bestehende eigene Gruppen bekommen es, wenn sie das Recht „Personen“ haben. Höchstens 300 Einträge pro Bereich. Anlegen, Ändern und Löschen stehen im Log (Bereich „Archiv“).

## Notfall UMBRA (Dialogkette)
„Notfall vorbereiten“ öffnet drei Fenster: Hinweis (GITs nötig) → Bestätigung mit deinem Namen → Meldung „/git <Name> autorisiert Notfallprotokoll Umbra und sperrt die Jedi Datenbank …“. Erst bei „Sicher“ wird gesperrt. Die Scan-Funktion bei den Planeten wurde entfernt.

## Berichte deklassifizieren
- Die Jedi Leitung vergibt unter Konten den Knopf „Deklassifizieren erlauben“ (Jedi Leitung darf immer).
- Im Bericht: „Deklassifizieren“ → der Deklassifizierer legt ein Passwort (8–100 Zeichen) fest und erhält den Link `/d/<Token>`.
- Abruf nur mit Link UND Passwort (öffentliche Seite `public/d.html`, ohne Anhänge, Schwärzungen aufgehoben, Stand eingefroren). Widerruf jederzeit; beim Löschen des Berichts erlischt der Link. Falsche Versuche werden begrenzt, bei Notfall UMBRA ist der Abruf gesperrt. Passwörter stehen nie in den Logs.

**Schwärzungspflicht:** Wer nicht zur Jedi Leitung gehört, muss einen Bericht beim Deklassifizieren eigenhändig schwärzen (mindestens eine Stelle in [[doppelten Klammern]], Text sonst unverändert). Die geschwärzten Stellen erscheinen öffentlich als ████████. Nur die Jedi Leitung darf ohne Schwärzung deklassifizieren.

**Karte:** Mausrad zoomt die Galaxis Karte (bis 8x) zum Mauszeiger, mit gedrückter Maus verschiebt man den Ausschnitt, Doppelklick setzt zurück. Bei normaler Größe scrollt das Rad weiter die Seite.
**Karte erweitern:** Herauszoomen (bis 0,25x) zeigt freien Raum rund um die Karte; im Bearbeitungsmodus lassen sich Systeme auch außerhalb des bisherigen Rahmens setzen (gestrichelter Rahmen = ursprünglicher Bereich).

**Dossier-Layout (Jedi):** Berichte, Berichts-Anhänge, Personenakten und der Austausch erscheinen im grauen Dossier-Layout (Kopf, Meta-Raster, nummerierte Abschnitte). Abschnitte im Text mit „## Überschrift“ beginnen.

**Akten-Nummer:** Jede Akte bzw. jeder Bericht im Jedi-Layout trägt eine Nummer „JA-“ plus 9 Zeichen (Buchstaben und Zahlen), die aus der internen ID erzeugt wird und gleich bleibt. Das Layout gilt auch für die Einträge der Jedi Ermittler.

**Aktive Aufträge:** Reiter mit den Tab „Beschattungen“. Er listet alle Personen aus dem Register, die beschattet werden, mit einem Textfeld „Auftrag / Begründung“ (Recht „Personen“ zum Schreiben). Beenden geht direkt dort oder bei der Person.

**JiA (Jedi im Auftrag):** In „Aktive Aufträge“ kann jedes Konto einen laufenden Auftrag („Beschattung“) übernehmen und erscheint dann mit seinem Namen als „JiA Name“ (auch in der Personenansicht). Mit dem Ende des Auftrags werden die Übernahmen gelöscht.

**Aufträge zuteilen:** Die Jedi Leitung teilt unter „Aktive Aufträge“ Konten einem Auftrag zu; zugeteilte Konten sind die Auftragsleitung (AL). Alle anderen Konten stellen mit „Übernahme anfragen“ eine Anfrage; die Auftragsleitung (und die Jedi Leitung) sieht die offenen Anfragen und kann sie „Akzeptieren“ oder „Ablehnen“. Erst akzeptierte Konten erscheinen als JiA.

**Sonstige Aufträge:** Tab in „Aktive Aufträge“ mit den Zeilen Titel, Zielperson / Zielobjekt, Grund und Beschreibung des Auftrags. Erstellen und Bearbeiten: Recht „Op. starten“ (fremde Aufträge: „Fremde Inhalte“). Zuteilen, Anfragen und Akzeptieren (JiA, Auftragsleitung) funktionieren wie bei der Beschattung.

**Recht „Beschatten“ und Dokumentation:** Gruppen können unter Konto → Gruppen das Recht „Beschatten“ bekommen (Personen auf Beschattung setzen, aufheben und den Auftrag schreiben, ohne Akten ändern zu dürfen). Mitglieder eines Auftrags (akzeptierte JiA, Auftragsleitung, Jedi Leitung) dokumentieren in „Aktive Aufträge“ Gespräche und Protokolle. Lesen dürfen nur Mitglieder und die Jedi Leitung; löschen kann der Verfasser oder die Auftragsleitung. Beim Beenden eines Auftrags bleibt die Dokumentation erhalten.

**Überwacher:** Konten der Gruppen „Wissensrat“ und „Jedi Leitung“ können unter „Aktive Aufträge“ mit „Auftrag überwachen“ Überwacher eines Auftrags werden. Sie sehen die Dokumentation (Gespräche, Protokolle) nur lesend, sind aber keine JiA und keine Auftragsleitung. Entfernen kann nur die Jedi Leitung.

**Geheimhaltung der Dokumentation:** Gespräche und Protokolle sieht nur, wer Teil des Auftrags ist (akzeptierte JiA, Auftragsleitung oder Überwacher). Die Jedi Leitung sieht die Dokumentation aller Aufträge immer und weist Überwacher zu (Auswahl „Überwacher zuweisen“ bei jedem Auftrag).

**Einstellungen (pro Konto):** Der Reiter „Einstellungen“ steht immer ganz unten. Jedes Konto wählt für sich die Farbe (Vorgaben oder eigene), die Darstellung (Minimalistisch, Normal, Groß) sowie Reihenfolge und Sichtbarkeit der Reiter. Die Werte werden am Konto gespeichert und gelten auf jedem Gerät.

## Zusatzfelder bei Berichten
Beim Erstellen/Bearbeiten eines Berichts können bis zu 20 Zusatzfelder mit selbst gewähltem Titel hinzugefügt werden ("+ Zusatzfeld hinzufügen"). Sie erscheinen in der Akte unter den Kopfdaten. `[[...]]` wird auch dort geschwärzt. Bei Deklassifizierung werden Zusatzfelder nur veröffentlicht, wenn die Jedi Leitung deklassifiziert; bei anderen Deklassifizierenden werden sie nicht veröffentlicht.

### Schwärzen-Knopf & Zusatzfelder deklassifizieren
Im Deklassifizieren-Formular Text im Berichtstext oder in einem Zusatzfeld markieren und auf „▮ Schwärzen“ klicken (setzt `[[…]]`). „Schwärzung aufheben“ entfernt sie wieder. Zusatzfelder werden jetzt für alle Deklassifizierenden veröffentlicht, mit denselben Schwärzungen; ihr Inhalt darf dabei nicht verändert werden.

## Mehr Anpassungen (Einstellungen)
Pro Konto einstellbar: Farbwelt (Schwarz, Graphit, Nachtblau), hoher Kontrast, Schriftart (Chakra Petch, Systemschrift, Monospace), Größe 80–140 %, Ecken (eckig, normal, rund), Effekte einzeln (Sternenhimmel, Laufband, Scanlinien, Animationen reduzieren, volle Breite, Start-Animation überspringen), Startseite nach der Anmeldung, sowie „Alle Einstellungen zurücksetzen“. Gespeichert am Konto (`/api/prefs`).

### Freie Farben
Akzentfarbe (Farbfeld oder Hex-Wert) färbt jetzt Kopfzeilen, Knöpfe, Rahmen, Auswahlfelder und Schein; die Hintergrundfarbe ist frei wählbar (`bg`), Flächen und Rahmen leiten sich davon ab.

## Entfernt
- **Wächter ID / Ordensmitglied ID:** gibt es nicht mehr. Konten haben nur noch einen Namen; Autoren, Deklassifizierer und Logs zeigen den Namen. Alte IDs werden beim Start aus den gespeicherten Daten gelöscht.
- **KoS (Kill on Sight):** Funktion, Tab, Banner, Zähler und Dokumentationsart „Eliminierung“ sind entfernt. Alte KoS-Daten werden beim Start gelöscht (bereits geschriebene Dokumentationen der Art „Eliminierung“ bei Beschattungen/Aufträgen bleiben lesbar).
- **RIS Kooperation:** Reiter, Berichtsaustausch, RIS-Konten und das Gruppenrecht „RIS Kooperation“ sind entfernt. Alte RIS-Konten und -Berichte werden beim Start gelöscht.
- „Wächter“ heißt überall „Jedi“ (z. B. „Jedi Datenbank“, „JiA“ = Jedi im Auftrag).
