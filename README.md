# Table control

This table control is based on tabulator

//German Version
1.	Überblick
Im Folgenden finden Sie ein weiteres Anwendungsbeispiel für das Erstellen einer eigenen Custom Web Control.
Für das folgende Beispiel wird die JavaScript Bibliothek „Tabulator" verwendet und mit Code für dessen Verwendung in WinCC Unified ergänzt.
 

2.	Funktionsweise
Das fertiggestellte Custom Web Control besitzt die folgenden Funktionalitäten:
•	Erstellen einer Tabelle anhand von Übergebenen WinCC Unified Variablenwerten
•	Abspeichern aller Werte in der Tabelle in einem String in WinCC Unified


3.	Use Case
Mögliche Verwendungszwecke:
•	.csv Datei liegt im TIA Projekt vor und soll in der Runtime angezeigt werden
•	Datensätze aus Maschinen anzeigen
•	Anzeigen von Datenbanksätzen

4.	Verwendete Komponenten
Dieses Anwendungsbeispiel wurde zusätzlich zu den bereits gezeigten Hard- und Softwarekomponenten, mit folgender Bibliothek erstellt erstellt: 

Komponente	Anzahl	Artikelnummer	Hinweis
Tabulator v4.9	1	Siehe Internet \5\
Open Source-Komponente – nicht im Beispiel-Code enthalten.
Laden Sie sich die Datei und kopieren Sie die Datei in das entsprechende Verzeichnis (siehe Kapitel 3.2 Engineering).

5	Konfiguration und Implementierung des Custom Web Controls
Dieses Kapitel zeigt, wie das Tabellen Custom Web Control mit Visual Studio Code erstellt wird.

	Custom Web Control erstellen
	1.	Ordnerstruktur erstellen
	(Siehe auch im Handbuch unter: „Allgemeiner Aufbau und Ordnerstruktur“)
	a.	Öffnen Sie den Windows Explorer und erstellen Sie in einem Arbeitsordner Ihrer Wahl die folgenden Elemente:
	b.	Datei mit Namen „manifest.json“ (Diese Datei bedarf einer bestimmten Struktur, die im Handbuch näher erklärt ist. Für dieses Applikationsbeispiel genügt das Kopieren einer existierenden Datei und eventuell ein paar Anpassungen)
	c.	Ordner mit Namen „assets“: 
	Platzieren Sie hier ein Icon in beliebigem Bildformat, welches im TIA Portal für dieses Control angezeigt werden soll.
	d.	Ordner mit Namen „control“: 
	Erstellen Sie hier eine Datei „index.html“ und fügen Sie die Datei „webcc.min.js“ aus den angehängten Dateien ebenfalls in dieses Verzeichnis hinzu
	Entpacken Sie die heruntergeladene „Tabulator“ Bibliothek ebenfalls in den Ordner „control“.
	(Quelle: https://github.com/olifolkerd/tabulator)

	2.	Visual Studio Code
	a.	Öffnen Sie Visual Studio Code oder einen anderen Editor Ihrer Wahl
	b.	Öffnen Sie den Ordner, in dem sich die soeben erstellte Ordnerstruktur befindet, über “File““Open Folder“.

	 
	3.	Manifest.json-Datei erstellen  
	a.	Öffnen Sie die Datei manifest.json.
	b.	Kopieren Sie den Inhalt der angehängten manifest.json-Datei in Ihre manifest.json.
	c.	Richten Sie Ihr manifest ein, wie bereits in Kapitel 2.1 beschrieben
	d.	Lassen Sie die Property „TableDataString“ unverändert. Hier wird der Inhalt der Tabelle zurückgeschrieben.

	4.	Index.html 
	a.	Zuerst wird auf notwendigen Scripte referenziert.
	b.	Im Anschluss erfolgt der Verbindungsaufbau mit der Funktion WebCC.start();
	c.	Direkt nach der Verbindung wird der aktuelle Wert, der sich hinter „TableDataString“ verbirgt in das Array „ArrayData“ geschrieben. 
	d.	Im Anschluss befindet sich die „subscribe“ Funktion. Hier wird überprüft ob sich der Wert von „TableDataString“ ändert, was beim Laden der Seite automatisch passiert. Beim Neuladen der Seite wird der Wert auf den „Static“ Wert aus dem TIA-Projekt gesetzt. Sobald dies passiert, wird der Wert wieder mit den aktuellen Werten aus „ArrayData“ überschrieben.

	5.	Arbeiten mit Tabulator 
	a.	In dem Feld „ArrayData“ stecken nun die Datenpunkte für die Tabelle. Diese können nun verwendet werden, um Tabellen mit Hilfe der Tabulator Funktionalitäten zu erstellen. (Für mehr Informationen siehe http://tabulator.info/)
	b.	Die Eigenschaften der Spalten der Tabelle über die Schnittstellen Variable „ColumnStyleString“ festgelegt.

	6.	Deploy Prozess und Installation 
	a.	Sind Sie fertig mit der Programmierung, erstellen Sie wie im vorherigen Beispiel eine .zip-Datei, die die Ordner „assets“, „control“ und die Datei „manifest.json“ beinhaltet. Diese .zip-Datei benötigt nun ebenfalls Ihren eigenen GUID-Namen. ({xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}.zip).
	b.	Machen Sie diese Datei nun für Ihr Anwender Projekt verfügbar indem Sie es in den „CustomControls“ Ordner Ihres Projektes kopieren.
	…Project_1\UserFiles\
	CustomControls\{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}.zip 
	c.	Öffnen Sie Ihr TIA-Projekt und ziehen Sie Ihre Custom Web Control auf Ihren gewünschten Screen. 
	d.	Verknüpfen Sie hier dann Ihr festgelegtes Interface mit Ihren HMI-Tags. (Die Interface Variable „TableDataString“ darf nicht „read only“ sein, da hier immer der aktuelle Tabellen Inhalt zurückgeschrieben wird.) 
	e.	Downloaden Sie Ihr Projekt nun auf die Runtime und verwenden Sie Ihre Tabelle.

6. Erstellen einer Tabelle
Zum Erstellen einer Tabelle müssen zwei Strings an die Custom Web Control übergeben werden:
	•	TableDataString: Ein String im JSON Format in dem die Werte der anzuzeigenden Tabelle stehen. Hier gilt, „field“ der Tabellen Spalte gefolgt von einem „:“ mit Wert. Beispiel:
	‘[{"name":"Donald","salary":"2000","onvacation":"true", “rating”:4}, {"name":"Mickey","salary":"2100","onvacation":"true", “rating”:2},…]’
	•	ColumnStyleString: Ebenfalls ein String im JSON Format, der die Eigenschaften der Spalten angibt.
Es folgt ein Beispiel Ausschnitt mit Erklärung:
"[{"title":"Name", "field":"name", "sorter":"string", "width":150, "headerFilter":"input"},..]"
	•	title: Gibt den Namen der Spalten an der auch in der Spalte der Tabelle gezeigt wird.
	•	field: Das „field“ dient der Zuordnung der Werte bei der Befüllung der Tabelle.
	•	sorter: Gibt an wonach sortiert wird.
	•	width: Gibt die Breite der Spalte an.
	•	headerFilter: Erstellt einen Filter am Kopf der Spalte.
Weitere Informationen zu den Eigenschaften finden Sie unter http://tabulator.info/


////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

//english Version
1.	Overview
n the following you will find another application example for creating your own custom web control.
For the following example the JavaScript library "Tabulator" is used and supplemented with code for its use in WinCC Unified.

2.	Functionality
The completed Custom Web Control has the following functionalities:
• Create a table based on transferred WinCC Unified tag values
• Saving of all values ​​in the table in a string in WinCC Unified


3.	Use Cases
Possible uses:
• .csv file is in the TIA project and should be displayed in runtime
• View records from machines
• Show data from a Database

4.	Used components
Tabulator Library - https://github.com/olifolkerd/tabulator

5.	Create the Custom Web Control
This chapter shows how the Custom Web Control tables are created with Visual Studio Code.

	Create custom web control
	1. Create folder structure
	(See also in the manual under: "General structure and folder structure")
	a. Open Windows Explorer and create the following items in a working folder of your choice:
	b. File with the name "manifest.json" (This file requires a certain structure, which is explained in more detail in the manual. For this application example, copying an existing file and possibly a few adjustments is sufficient)
	c. Folder named "assets":
	Place an icon here in any image format that is to be displayed in the TIA Portal for this control.
	d. Folder named "control":
	Create a file "index.html" here and add the file "webcc.min.js" from the attached files to this directory as well
	Unzip the downloaded "tab" library into the "control" folder.
	(Source: https://github.com/olifolkerd/tabulator)

	2. Visual Studio Code
	a. Open Visual Studio Code or another editor of your choice
	b. Open the folder in which the folder structure you have just created is located via “File“  “Open Folder“.

 
	3. Create the Manifest.json file
	a. Open the manifest.json file.
	b. Copy the contents of the attached manifest.json file into your manifest.json.
	c. Set up your manifest as already described in chapter 2.1
	d. Leave the "TableDataString" property unchanged. The content of the table is written back here.

	4. Index.html
	a. First, references are made to the necessary scripts.
	b. The connection is then established with the WebCC.start () function;
	c. Immediately after the connection, the current value, which is hidden behind “TableDataString”, is written into the “ArrayData” array.
	d. Then there is the "subscribe" function. Here it is checked whether the value of "TableDataString" changes, which happens automatically when the page is loaded. When the page is reloaded, the value is set to the "Static" value from the TIA project. As soon as this happens, the value is overwritten again with the current values ​​from "ArrayData".

	5. Working with Tabulator
	a. The data points for the table are now in the "ArrayData" field. These can now be used to create tables with the help of the tabulator functionalities. (For more information see http://tabulator.info/)
	b. The properties of the table columns are defined using the “ColumnStyleString” interface tag.
	
	6. Deploy process and installation
	a. When you have finished programming, create a .zip file as in the previous example, which contains the folders “assets”, “control” and the file “manifest.json”. This .zip file now also needs your own GUID name. ({xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx} .zip).
	b. Make this file available for your user project by copying it into the “CustomControls” folder of your project.
	… Project_1 \ UserFiles \
	CustomControls \ {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx} .zip
	c. Open your TIA project and drag your Custom Web Control onto your desired screen.
	d. Then link your defined interface with your HMI tags here. (The interface variable “TableDataString” must not be “read only”, since the current table content is always written back here.)
	e. Now download your project to Runtime and use your table.
	
6. Create a table
To create a table, two strings must be transferred to the Custom Web Control:
	• TableDataString: A string in JSON format containing the values ​​of the table to be displayed. The following applies here: “field” of the table column followed by a “:” with value. Example:
	'[{"name": "Donald", "salary": "2000", "onvacation": "true", “rating”: 4}, {"name": "Mickey", "salary": "2100" , "onvacation": "true", “rating”: 2},…] '
	• ColumnStyleString: Also a string in JSON format that specifies the properties of the columns.

	An example excerpt with explanation follows:
"[{"title":"Name", "field":"name", "sorter":"string", "width":150, "headerFilter":"input"},..]"
	• title: Specifies the name of the column that is also shown in the column of the table.
	• field: The "field" is used to assign the values ​​when filling the table.
	• sorter: Specifies according to what sorting is carried out.
	• width: Indicates the width of the column.
	• headerFilter: Creates a filter at the head of the column.
More information about the properties can be found at http://tabulator.info/