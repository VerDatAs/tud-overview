# Übersicht der TUD (WIP)

Dieses Projekt gibt einen Überblick über die von der TU Dresden entwickelten Komponenten und veröffentlichten Dokumente für das tutorielle Assistenzsystem (TAS), welches im Rahmen des Forschungsprojekts VerDatAs in interdisziplinärer Zusammenarbeit von Psychologie und Informatik konzipiert, umgesetzt und evaluiert wurde.

## Überblick der Komponenten

Die folgenden technischen Komponenten wurden entwickelt:

* [tud-assistance-backbone](https://github.com/VerDatAs/tud-assistance-backbone): Die Softwarekomponente, die zur Analyse von Lernverlaufsdaten in Form von xAPI-Statements verwendet wird und Rückmeldungen zu den Interaktionen der Lernenden sowie Hilfestellungen und Vorschläge zum Lernstand und den entsprechenden Lerninhalten liefert.
* [tud-tas-backend](https://github.com/VerDatAs/tud-tas-backend): Die Softwarekomponente, die als Adapter zwischen dem TUD Assistance Backbone und dem Lernmanagementsystem ILIAS fungiert.
* [tud-dashboard-plugin](https://github.com/VerDatAs/tud-dashboard-plugin): Das ILIAS-Plugin (PageComponent) zur Einbindung eines Dashboards als eine der Client-Anwendungen des TAS, welches eine gebaute Version des [tud-dashboard](https://github.com/VerDatAs/tud-dashboard) integriert, um eine grafische Wissensstruktur zu realisieren.
* [tud-chatbot-plugin](https://github.com/VerDatAs/tud-chatbot-plugin): Das ILIAS-Plugin (UserInterfaceHook) zur Einbindung eines Chatbots als eine der Client-Anwendungen des TAS, welches eine gebaute Version des [tud-chatbot](https://github.com/VerDatAs/tud-chatbot) integriert, um Feedbackprozesse für Nutzende abzubilden.
* [tud-evaluation-plugin](https://github.com/VerDatAs/tud-evaluation-plugin): Das ILIAS-Plugin (UserInterfaceHook) zur Einbindung einer Evaluationskomponente als eine der Client-Anwendungen des TAS, welches Statistiken für Nutzende bereitstellt (aktuell hauptsächlich für den Kurstutor bzw. -administrator).

### Setup Informationen

Die Informationen zum Setup des TAS und der zugehörigen Komponenten können über [folgendes Dokument](SETUP.md) abgerufen werden.

## Veröffentlichte Dokumente

Die im Rahmen von VerDatAs erstellen Dokumente der TU Dresden wurden über Zenodo unter der [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0) Lizenz veröffentlicht ([https://zenodo.org/communities/verdatas/records](https://zenodo.org/communities/verdatas/records)) und werden nachfolgend zusammengefasst:

* [Anchored Instructions](https://doi.org/10.5281/zenodo.11064775): Diese Anleitung soll Autoren bzw. Lehrende dabei unterstützen, Ankersituationen für ihre Online-Kurse besser zu verstehen und Anker selbständig für ihr Lernmaterial zu erstellen.
* [Anleitung Aufgabenkonstruktion](https://doi.org/10.5281/zenodo.11190395): Die Konstruktion von Lernaufgaben ist ein entscheidender Aspekt in der Gestaltung von (Online-)Kursen. Diese Anleitung soll Lehrpersonen dabei helfen, zielgerichtet und praxisnah zu vermitteln. In diesem Dokument werden 6 Schritte vorgestellt, um Aufgaben innerhalb von (Online-)Kursen zu konstruieren.
* [Anleitung Kooperationsaufgaben](https://doi.org/10.5281/zenodo.11190452): Diese Anleitung soll Lehrpersonen helfen, Kooperationsaufgaben zu verstehen und mit Hilfe einer Checkliste die Erstellung zu vereinfachen.
* [Lernpfade erstellen](https://doi.org/10.5281/zenodo.11190631): Diese Anleitung gibt Antworten auf folgende Fragen:
  * Was sind Lernpfade und wie sind sie aufgebaut?
  * Welche Vorteile haben sie?
  * Welche Gestaltungskriterien sind hilfreich und wie werden sie erstellt?
* [Schlüsselprinzipien des Blended Learnings - eine Zusammenfassung zweier Studien](https://doi.org/10.5281/zenodo.11191209): Diese Zusammenfassung beleuchtet die Bedeutung von Blended Learning-Methoden für ein erfolgreiches Lernumfeld.
* [Gestaltung von Erklärvideos](https://doi.org/10.5281/zenodo.11191837): In dieser Anleitung wird auf die Merkmale von Erklärvideos eingegangen und Tipps zur Erstellung sollen Autor*innen unterstützen, eigene Videos zu erstellen.
* [Strukturierung von Lerninhalten](https://doi.org/10.5281/zenodo.11235399): Diese Anleitung zur Strukturierung von Lerninhalten soll Strukturierungsinformationen auf Basis von wissenschaftlichen Erkenntnissen liefern, um diese gezielt anwenden zu können. Dabei geht es zum einen um die grundsätzliche Strukturierung von Lerninhalten, andererseits aber auch um die Aufbereitung und Präsentation von Lernaufgaben.
* [Dialoggestaltung eines Chatbots](https://doi.org/10.5281/zenodo.11235522): Mit diesem Dokument wird ein Vorgehen beschrieben, wie man bei der Gestaltung eines Chatbots vorgehen kann und welche Komponenten und Überlegungen eine Rolle spielen, um einen nutzerzentrierten Chatbot zu entwickeln. 
* [Lernverlaufsdaten: Welche Unterstützungsszenarien wurden im VerDatAs Projekt realisiert](https://doi.org/10.5281/zenodo.11367079): In diesem Dokument wird ein Überblick über die realisierten Unterstützungsmechanismen gegeben, indem sowohl die Trigger, die Lernverlaufsdaten als auch die Nachrichten bzw. Empfehlungen tabellarisch aufgelistet werden.
* [VerDatAs: Auswertung der technischen Evaluation vom 10.04.2024](https://doi.org/10.5281/zenodo.11174279): In diesem Dokument wird eine technische Evaluation des tutoriellen Assistenzsystems ausgewertet. Dadurch sollen die im Rahmen der Evaluation erzielten Ergebnisse auch für Dritte nachvollziehbar aufgelistet werden.
