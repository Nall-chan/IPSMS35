# IPSMS35
IPS-Modul f�r den Conrad MS35 RGB-Controller.  

**Dokumentation**

**Inhaltsverzeichnis**

1.Funktionsumfang 1

2.Voraussetzungen 2

3.Software-Installation 2

4.Hardware-Installation & Einrichtung 2

5.Einrichten der Instanzen in IPS 3

6.Statusvariablen und Profile 3

7.WebFront 4

8.PHP-Befehlsreferenz 4

9.Parameter / Modul-Infos 6

10.Tips & Tricks 6

11.Anhang 6

1.  **Funktionsumfang**

    Direkte native Unterst�tzung des Conrad MS-35 RGB-Controller (EAN: 4016138567267 Bestellnr.: 181818 ).

-   Setzen einer Farbe.

-   Starten eines der neun internen Programme:

    -   3x verschiedene Farbwechsel (Programm 1-3)

    -   Gewitter (Programm 4)

    -   Kaminfeuer (Programm 5)

    -   Sonnenauf- & untergang (Programm 6)

    -   Farbblitze (Programm 7)

    -   2x Benutzerspezifisch (Programm 8 & 9)

-   Setzen der Helligkeit (gilt nur f�r Programme).

-   Setzen der Ablauf-Geschwindigkeit (gilt nur f�r Programme; nicht m�glich bei Gewitter und Kaminfeuer).

-   Pause & Fortsetzen des aktiven Programms.

-   Ein- und Ausschalten (Aus = dunkel-gesteuert; Ein = Initialisierung der Parameter wie nach Spannungswiederkehr.

-   Programmieren der benutzerspezifischen Programme.

1.  **Voraussetzungen**

    - IPS ab Version 3.1 unter Windows

    - MS-35 RGB-Controller

    - RS232-Schnittstelle auf TTL-Basis (oder jede andere Form der seriellen Datenanbindung mit 5V; z.B. XBee mit TTL-Adapterplatine)

2.  **Software-Installation**

    **
    **Kopieren von der MS35.dll in das Unterverzeichnis 'modules' unterhalb des
    IP-Symcon Installationsverzeichnisses.
    Der Ordner 'modules' muss u.U. manuell angelegt werden.
    Beispiel: 'C:\\IP-Symcon\\modules'
    IPS-Dienst Neustarten.

3.  **Hardware-Installation & Einrichtung**

    ![](Doku_html_m4b3399bc.png)**
    **Den Controller gem�� Handbuch beschalten.

    ![](Doku_html_m47910d47.png)Die serielle Verbindung z.B. mit dem Programmierkabel (oder andere jede Art einer seriellen Anbindung) herstellen.

4.  ![](Doku_html_m1ed1e14.png)**Einrichten der Instanzen in IPS**

    **
    **Unter Instanz hinzuf�gen ist der 'MS35 RGB-Controller' unter dem Hersteller 'Conrad' aufgef�hrt.

    Es wird automatisch ein SerialPort angelegt.
    Die Einstellungen des SerialPort sind auf 38000 Baud zu konfigurieren. Die restlichen Parameter bleiben auf den Standardwerten 8 Datenbits, 1 Stopbit, keine Parit�t.

    Wird eine andere Hardware zur Daten�bertragung genutzt, ist diese ebenfalls auf diese Parameter zu konfigurieren und die SerialPort-Instanz zu l�schen.

    Die Instanz der MS35 ben�tigt keine eigene Konfiguration.

    Daf�r wurde das Testcenter umgesetzt, mit dem die Funktion sofort �berpr�ft werden kann.

5.  **Statusvariablen und Profile**

    ![](Doku_html_74a518cb.png)

    **<span style="font-weight: normal">Die Statusvariablen werden f�r jeden Controller automatisch angelegt. L�schen kann zu Fehlfunktionen f�hren; da Sie z.B. f�r das ausf�hren eines Farb-Programms ben�tigt werden. Umbenennen ist nat�rlich kein Problem.</span>**

    **<span style="font-weight: normal">Definition:</span>**

    **<span style="font-weight: normal">- STATE = Status des Controllers als boolescher Wert true = An; false = Aus;</span>**

    **<span style="font-weight: normal">- Color = Aktueller Farbwert (int) , wenn kein Programm l�uft.</span>**

    **<span style="font-weight: normal">- Program = Aktuell aktives Programm. (int) 1-9</span>**

    **<span style="font-weight: normal">- Play = Status der Programmausf�hrung (int) 1 = Play; 2 = Pause; 3 = Stop</span>**

    **<span style="font-weight: normal">- Brightness = Helligkeit bei Programmausf�hrung (int) 1=normal; 2 = mittel; 3 = dunkel</span>**

    **<span style="font-weight: normal">- Speed = Geschwindigkeit bei Programmausf�hrung (int) 1,2,4,8,16,32,64,128 fache</span>**

**<span style="font-weight: normal"> Verlangsamung.</span>**

1.  **WebFront**

    **<span style="font-weight: normal">Der Controller kann direkt �ber das WebFront bedient werden, ohne das weitere erstellen von Scripten.</span>**

    **<span style="font-weight: normal">Es ist f�r alle Statusvariablen eine Standardaktion hinterlegt, welche sich direkt auf den Controller auswirkt. Dies kann auf Wunsch auch unter dem Reiter 'Statusvariablen' der MS35-Instanz, deaktiviert werden.</span>**

    ![](Doku_html_7c4200a.png)

2.  **PHP-Befehlsreferenz**

    **<span style="font-weight: normal">boolean MS35\_SetRGB(integer $InstanzeID, integer $Red, integer $Green, integer $Blue);</span>**

    **<span style="font-weight: normal">boolean MS35\_Switch(integer $InstanzeID, boolean State);</span>**

    **<span style="font-weight: normal">boolean MS35\_Play(integer $InstanzeID);</span>**

    **<span style="font-weight: normal">boolean MS35\_Pause(integer $InstanzeID);</span>**

    **<span style="font-weight: normal">boolean MS35\_Stop(integer $InstanzeID);</span>**

    **<span style="font-weight: normal">boolean MS35\_RunProgram(integer $InstanzeID, integer $Program);</span>**

    **<span style="font-weight: normal">string MS35\_SetSpeed(integer $InstanzeID, integer $Speed);</span>**

    **<span style="font-weight: normal">string MS35\_SetBrightness(integer $InstanzeID, integer $Brightness);</span>**

    **<span style="font-weight: normal">string MS35\_SetProgram(integer $InstanzeID, integer $Program, string $Data);</span>**

3.  **Parameter / Modul-Infos**

    **
    <span style="font-weight: normal">GUID</span><span lang="en-US"><span style="font-weight: normal">s</span></span><span style="font-weight: normal"> der Instanz</span><span style="font-weight: normal"> (z.B. wenn Instanz per PHP angelegt werden soll):</span>**

    **<span style="font-weight: normal">
    Eigenschaften f�r Get/SetProperty-Befehle:
     � entf�llt � </span>**

4.  **Tips & Tricks**

    **
    <span style="font-weight: normal">- Sollte das Ger�t mal nicht korrekt antworten, so wird bei der n�chsten Ausf�hrung eines Befehls versucht der Controller neu zu initialisieren. Welches einen Verlust der schon eingestellten Helligkeit und Geschwindigkeit bedeutet.</span>**

    **<span style="font-weight: normal">- Das Modul f�gt automatisch Zwangspausen in ms Bereich ein, wenn zu viele Befehle auf einmal �bertragen werden m�ssen (z.B. SetProgram). W�rde dies nicht passieren, kommt der Controller h�ufig aus dem Sync zur Schnittstelle und muss neu initialisiert werden. Bevor er auf Befehle wieder reagiert.</span>**

    **<span style="font-weight: normal">- SetProgram kann maximal 51 Squenzen aufnehmen und im Controller abspeichern. Diese �bertragung dauert Zeit. Im Zweifelsfall ist die maximal Ausf�hrungszeit des Scriptes anzupassen.</span>**

    **<span style="font-weight: normal">Folgender PHP-Code liefert </span>**ein**<span style="font-weight: normal"> </span>**Beispiel**<span style="font-weight: normal"> wie man den JSON-String mit dem korrekten Aufbau, erzeugen kann:</span>**

    `$Sequenz\['R'\] = 0x00;
    $Sequenz\['G'\] = 0xFF;
    $Sequenz\['B'\] = 0xFF;
    $Sequenz\['H'\] = 0x05;
    $Sequenz\['F'\] = 0x05;
    $Data\[\] = $Sequenz;
    $Sequenz\['R'\] = 0xFF;
    $Sequenz\['G'\] = 0x00;
    $Sequenz\['B'\] = 0xFF;
    $Sequenz\['H'\] = 0x05;
    $Sequenz\['F'\] = 0x05;
    $Data\[\] = $Sequenz;
    MS35\_SetProgram(123456 /\* Controller \*/, 8, json\_encode($Data));` 

5.  **Anhang**

    **<span style="font-weight: normal">
    Changlog:
    1.1. : Erstes �ffentliches Release mit SetProgram</span>**


