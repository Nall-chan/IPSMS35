# IPSMS35
IPS-Modul f�r den Conrad MS35 RGB-Controller.  

## Inhaltsverzeichnis

1. [Funktionsumfang](#1-funktionsumfang)

2. [Voraussetzungen](#2-voraussetzungen)

3. [Software-Installation](#3-software-installation)

4. [Hardware-Installation & Einrichtung](#4-hardware-installation--einrichtung)

5. [Einrichten der Instanzen in IPS](#5-einrichten-der-instanzen-in-ips)

6. [Statusvariablen und Profile](#6-statusvariablen-und-profile)

7. [WebFront](#7-webfront)

8. [PHP-Befehlsreferenz](#8-php-befehlsreferenz)

9. [Parameter / Modul-Infos](#9-parameter--modul-infos)

10. [Tips & Tricks](#10-tips--tricks)

11. [Anhang](#11-anhang) 

## 1. Funktionsumfang

   Direkte native Unterst�tzung des Conrad MS-35 RGB-Controller (EAN: 4016138567267 Bestellnr.: 181818 ).

   *   Setzen einer Farbe.  

   *   Starten eines der neun internen Programme:

      -   3x verschiedene Farbwechsel (Programm 1-3)

      -   Gewitter (Programm 4)

      -   Kaminfeuer (Programm 5)

      -   Sonnenauf- & untergang (Programm 6)

      -   Farbblitze (Programm 7)

      -   2x Benutzerspezifisch (Programm 8 & 9)

   *   Setzen der Helligkeit (gilt nur f�r Programme).

   *   Setzen der Ablauf-Geschwindigkeit (gilt nur f�r Programme; nicht m�glich bei Gewitter und Kaminfeuer).

   *   Pause & Fortsetzen des aktiven Programms.

   *   Ein- und Ausschalten (Aus = dunkel-gesteuert; Ein = Initialisierung der Parameter wie nach Spannungswiederkehr.

   *   Programmieren der benutzerspezifischen Programme.

## 2. Voraussetzungen

   - IPS ab Version 3.1 oder Version 4.x

   - MS-35 RGB-Controller

   - RS232-Schnittstelle auf TTL-Basis (oder jede andere Form der seriellen Datenanbindung mit 5V; z.B. XBee mit TTL-Adapterplatine)

## 3. Software-Installation

**IPS 3.1 bis 3.4:**  
   Bei GitHub das gepackte Modul als ZIP-File laden: [MS35.ZIP](https://github.com/Nall-chan/IPSMS35/raw/master/IPS3.X/MS35.zip)  
   Die enthaltende Datei MS35.dll in der /modules Verzeichnis von IPS kopieren.  
   Ist der Ordner Modules nicht vorhanden, so ist er anzulegen.  

**IPS 4.x:**  
   �ber das Modul-Control folgende URL hinzuf�gen.  
   `git://github.com/Nall-chan/IPSMS35.git`  

## 4. Hardware-Installation & Einrichtung

   ![](Doku/Doku_html_m4b3399bc.png)  
   Den Controller gem�� Handbuch beschalten.  
   ![](Doku/Doku_html_m47910d47.png)  
   Die serielle Verbindung z.B. mit dem Programmierkabel (oder andere jede Art einer seriellen Anbindung) herstellen.  

## 5. Einrichten der Instanzen in IPS

   Unter Instanz hinzuf�gen ist der 'MS35 RGB-Controller' unter dem Hersteller 'Conrad' aufgef�hrt.  
   Es wird automatisch ein SerialPort angelegt.  
   Die Einstellungen des SerialPort sind auf 38000 Baud zu konfigurieren. Die restlichen Parameter bleiben auf den Standardwerten 8 Datenbits, 1 Stopbit, keine Parit�t.  
   Wird eine andere Hardware zur Daten�bertragung genutzt, ist diese ebenfalls auf diese Parameter zu konfigurieren und die SerialPort-Instanz zu l�schen.  
   Die Instanz der MS35 ben�tigt keine eigene Konfiguration.  
   Daf�r wurde das Testcenter umgesetzt, mit dem die Funktion sofort �berpr�ft werden kann.  
   ![](Doku/Doku_html_m1ed1e14.png)  

## 6. Statusvariablen und Profile

   ![](Doku/Doku_html_74a518cb.png)

   Die Statusvariablen werden f�r jeden Controller automatisch angelegt. L�schen kann zu Fehlfunktionen f�hren; da Sie z.B. f�r das ausf�hren eines Farb-Programms ben�tigt werden. Umbenennen ist nat�rlich kein Problem.  
   Definition:  

   - STATE = Status des Controllers als boolescher Wert true = An; false = Aus;  
   - Color = Aktueller Farbwert (int) , wenn kein Programm l�uft.  
   - Program = Aktuell aktives Programm. (int) 1-9  
   - Play = Status der Programmausf�hrung (int) 1 = Play; 2 = Pause; 3 = Stop  
   - Brightness = Helligkeit bei Programmausf�hrung (int) 1=normal; 2 = mittel; 3 = dunkel  
   - Speed = Geschwindigkeit bei Programmausf�hrung (int) 1,2,4,8,16,32,64,128 fache Verlangsamung.  

   Die ben�tigten Profile werden ebenfalls automatisch angelegt und hei�en:  

   - MS35.PrgStatus (f�r die Statusvariable 'Play')  
   - MS35.Program (f�r die Statusvariable 'Program' � Enth�lt die Namen der Programme)  
   - MS35.Brightness (f�r die Statusvariable 'Brightness')  
   - MS35.Speed (f�r die Statusvariable 'Speed')  

   Die Profile k�nnen ver�ndert werden. Werden sie jedoch gel�scht; werden Sie automatisch neu angelegt.  

## 7. WebFront

   Der Controller kann direkt �ber das WebFront bedient werden, ohne das weitere erstellen von Scripten.  
   Es ist f�r alle Statusvariablen eine Standardaktion hinterlegt, welche sich direkt auf den Controller auswirkt. Dies kann auf Wunsch auch unter dem Reiter 'Statusvariablen' der MS35-Instanz, deaktiviert werden.  
   ![](Doku/Doku_html_7c4200a.png)  

## 8. PHP-Befehlsreferenz

   `boolean MS35_SetRGB(integer $InstanzeID, integer $Red, integer $Green, integer $Blue);`  
        Setzt die Farbwerte f�r Rot (Red), Gr�n (Green) und Blau (Blue). Ein laufendes Programm wird dadurch unterbrochen (Stop).  
        Erlaubte Werte f�r die Farben sind 0 bis 255.  
        Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

   `boolean MS35_Switch(integer $InstanzeID, boolean State);`  
        Schaltet den Controller aus oder ein.  
        Dabei wird das Ger�t nicht komplett abgeschaltet, da es sonst nicht mehr erreichbar w�re.  
        Aus ist hier das setzten der Farbe auf 0 (Alle Kan�le auf 0%).  
        Aus- / Einschalten setzt au�erdem alle Werte f�r Brightness und Speed auf die Werte wie nach Spannungswiederkehr.  
        Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

   `boolean MS35_Play(integer $InstanzeID);`  
        Das aktuell ausgew�hlte Programm wird fortgesetzt.  
	Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

   `boolean MS35_Pause(integer $InstanzeID);`  
        Das aktuell wiedergegebene Programm wird angehalten.  
	Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

   `boolean MS35_Stop(integer $InstanzeID);`  
        Das aktuell wiedergegebene Programm wird beendet.  
        Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

   `boolean MS35_RunProgram(integer $InstanzeID, integer $Program);`  
        Das Programm mit dem Index `$Program` wird wiedergegeben.  
	`$Program` muss zwischen 1 bis 9 liegen.  
        Eine �bersicht ist im ersten Kapitel.  
        Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

   `string MS35_SetSpeed(integer $InstanzeID, integer $Speed);`  
        Legt die Geschwindigkeit f�r die Ausf�hrung eines Programmes fest.  
        Kann vor oder nach RunProgramm aufgerufen werden.  
        Kann aber nicht zusammen mit den Programmen 4 & 5 verwendet werden.  
        Der Befehl wird dann ignoriert bzw. beim laden von Diesen Programmen auf 0 gesetzt.  
        `$Speed` muss dabei zwischen 0 und 8 liegen.  
        Wobei 0 normale Geschwindigkeit ist, und jede Stufe von 1-8 eine Halbierung der Geschwindigkeit ist (1/2, 1/4, 1/8, 1/16, usw.)  
        Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

   `string MS35_SetBrightness(integer $InstanzeID, integer $Brightness);`  
        Legt die Helligkeit f�r die Ausf�hrung eines Programmes fest.  
        Kann vor oder nach RunProgramm aufgerufen werden.  
        `$Brightness` muss dabei zwischen 1 und 3 liegen.  
        Wobei 1 der vollen, 2 der mittlere und 3 der niedrige Helligkeit entspricht.  
        Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

   `string MS35_SetProgram(integer $InstanzeID, integer $Program, string $Data);`  
        Schreibt eines der benutzerspezifischen Programme 8 oder 9 in den Controller.  
	`$Programm` darf nur 8 oder 9 enthalten.  
        `$Data` ist ein JSON-Codierter String welcher das Programm nach folgendem Schema enthalten muss:  
            (Beispiele im Kapitel 10)  
            `[{"R":255,"G":255,"B":255,"H":5,"F":5},{"R":0,"G":0,"B":255,"H":5,"F":5}]`  

   - R,G,B sind die Farbwerte der Kan�le von 0-255.  
   - H  ist die Haltezeit der Farbe von 0-255 x 0,13 Sek (Hold)  
   - F  ist die �berblendzeit von 0-255 x 0,13 Sek (Fade)  
            Es d�rfen maximal 51 dieser Sequenzen �bergeben werden.  
	Konnte der Befehl erfolgreich ausgef�hrt werden, liefert er als Ergebnis TRUE, andernfalls FALSE.  

## 9. Parameter / Modul-Infos

   GUID:
	{78EC291F-DD08-474C-950B-4EC547F31D26}

   Eigenschaften f�r Get/SetProperty-Befehle:
     � entf�llt �

## 10. Tips & Tricks

   - Sollte das Ger�t mal nicht korrekt antworten, so wird bei der n�chsten Ausf�hrung eines Befehls versucht der Controller neu zu initialisieren. Welches einen Verlust der schon eingestellten Helligkeit und Geschwindigkeit bedeutet.  
   - Das Modul f�gt automatisch Zwangspausen in ms Bereich ein, wenn zu viele Befehle auf einmal �bertragen werden m�ssen (z.B. SetProgram). W�rde dies nicht passieren, kommt der Controller h�ufig aus dem Sync zur Schnittstelle und muss neu initialisiert werden. Bevor er auf Befehle wieder reagiert.  
   - SetProgram kann maximal 51 Squenzen aufnehmen und im Controller abspeichern. Diese �bertragung dauert Zeit. Im Zweifelsfall ist die maximal Ausf�hrungszeit des Scriptes anzupassen.  

   Folgender PHP-Code liefert **ein** Beispiel wie man den JSON-String mit dem korrekten Aufbau, erzeugen kann:  

    $Sequenz['R'] = 0x00;  
    $Sequenz['G'] = 0xFF;  
    $Sequenz['B'] = 0xFF;  
    $Sequenz['H'] = 0x05;  
    $Sequenz['F'] = 0x05;  
    $Data[] = $Sequenz;  
    $Sequenz['R'] = 0xFF;  
    $Sequenz['G'] = 0x00;  
    $Sequenz['B'] = 0xFF;  
    $Sequenz['H'] = 0x05;  
    $Sequenz['F'] = 0x05;  
    $Data[] = $Sequenz;  
    MS35_SetProgram(123456 , 8, json_encode($Data));  

## 11. Anhang

   Changlog:  
   2.0. : Erstes (noch nicht endg�ltig getestetes) �ffentliches Release f�r IPS 4.0


