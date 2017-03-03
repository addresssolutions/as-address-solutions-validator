# as-address-solutions-validator

Namens- und Adressprüfung für serverseitiges javascript/nodejs

Achtung: Das Modul befindet sich derzeit noch in der Entwicklung! Bei Fragen, Anregungen, etc. bitte die u.g. Kontaktperson ansprechen!

Hinweis zur Nutzung:

Die Verwendung ist für Validierungs- und Testzwecke frei. Für den professionellen/komerziellen Gebrauch ist allerdings eine Lizenz und ein Nutzungscode erforderlich, der bei AS Address Solutions zu erwerben ist. Stellen wir eine unerlaubte und intensive Nutzung der Services durch einzelne Personen/Organisationen fest, wird der Dienst abgeschaltet. 

Features:

Dieses Modul bietet Schnittstellen zum Aufruf verschiedener Validierungsfunktionen für Namen und Adressen aus dem Hause AS Address Solutions GmbH. In der ersten Version werden die folgenden Funktionalitäten unterstützt:
- Validierung von Namenselementen (Anrede, Titel, Vorname, Nachname) auf Feldebene (z.B.: enthält ein Vornamensfeld tatsächlich einen gültigen Vornamen)
- Strukturierung von Namen und Verteilung von Namenselementen auf die vorgesehenen Felder
- Anredegenerierung und -korrektur
- Prüfung der postalischen Daten und Korrektur/Autoergänzung bei zweifelsfreier Rerferenz (z.B. "52223 Stollberg, Keiserplatz 6" korrigieren nach "52222 Stolberg, Kaiserplatz 6"
- Generieren von Auswahllisten für PLZ, Ort und Straßenname

Das Modul führt dabei einen XmlRpc-Request an einen Server der AS Address Solutions GmbH durch. Die Antwort erfolgt i.d.R. im Millisekundenbereich.

Unter https://github.com/addresssolutions/as-input-validator-sample ist eine komplette Beispielanwendung in nodejs zu finden.

Installation

Clonen des repository https://github.com/addresssolutions/as-address-solutions-validator.git oder einfach das ZIP-File downloaden und entpacken.

Funktionsaufrufe

Namensstrukturierung:

    // erzeugen eines Request-Objets vom TypASConvertNameRequest und Befüllung der Übergabeparameter
    var as_req = new as_address_solutions_validator.ASConvertNameRequest(
      1,                          // VersionInput, z.Zt. immer 1
      "de",                       // Countrycode de/nl/gb/fr/ch/nn
      <Anrede>,
      <Titel>,
      <Vorname>,
      <Nachname>);

    // Aufruf der Funktion 
    // bei erfolgreichem Aufruf ist error nicht belegt und response enthält ein json-objekt vom Typ ASConvertNameResponse
    // im Fehlerfall ist error belegt und der Responsetyp ist ASConvertNameError
    as_address_solutions_validator.ASConvertName(as_req, function (error, response) {
      if (error) {
        res.sendStatus(error.ErrorStatus);
      }
      else {
        return res.json(response);
      }
    });
 
Request-/Response-Typen
  
ASConvertNameRequest:
- VersionInput
- CountrycodeInput
- SalutationInput
- TitleInput
- FirstnameInput
- LastnameInput
    
ASConvertNameResponse:
- ReleaseInfo              // ReleaseInfo of used Server
- GeneralStatus            // general Status (1=everythin fine no conversion, 2=slight corrections, ...)
- ConversionMessage        // Text about conversion
- SalutationOutput         // corrected Salutation output
- SalutationOutputMessage  // message about wrong content in Salution field
- TitleOutput              // next fields same as Salution
- TitleOutputMessage
- FirstnameOutput
- FirstnameOutputMessage
- LastnameOutput
- LastnameOutputMessage
    
ASConvertNameError
- ErrorStatus              // technical error in call to ConvertBox (i.e. Network problem)
- ErrorMessage             // info text to ErrorStatus

Author

Ralf Geerken
http://www.address-solutions.de
r.geerken@address-solutions.de

Copyright and license

Copyright 2016 AS Address Solutions under the MIT license.
