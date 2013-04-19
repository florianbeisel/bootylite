Title: Postfächer exportieren mit Exchange 2007
tags: exchange 

*Dieser Artikel wurde 2012 auf pacey.me veröffentlicht*

Ich habe mir dieses Wochenende die Nächte um die Ohren geschlagen bei dem Versuch E-Mails und Benutzer aus einer bestehenden Microsoft Server 2003 (R2) Domäne (mit Exchange 2007 SP2) auf einen aktuellen Small Business Server 2011 zu migrieren. Besonders der Export der Mails erweist sich als unintuitiver als ich eigentlich von Microsoft gedacht hätte. Im folgenden nun meine aufgearbeiteten Notizen.

[cut]

## Grundlagen
Den einen oder anderen mag es wundern – für mich war es sehr verwunderlich, dass es so komplex ist E-Mails aus Exchange zu exportieren. In einigen Technet Artikeln habe ich die notwendigsten Grundinformationen gesammelt.

Hier meine grundlegenden Erkenntnisse:

* E-Mails können über die das PowerShell Cmdlet Export-Mailbox aus Exchange exportiert werden
* Der den Export ausführende Benutzer braucht Vollzugriff auf das Postfach.
* Export-Mailbox benötigt eine lokale Office Installation.

## Vorbereitungen

Wenn ich so über meinen eigenen Ablauf nachdenke bin ich in so ziemlich jedes Fettnäpfchen getreten, dass der Export zu bieten hat. Ich erspare euch jeden meiner Fehler und weise einfach auf die Fallstricke hin. Aufgrund der ganzen Abhängigkeiten würde ich empfehlen eine eigene virtuelle Maschine dafür einzurichten.

Als Betriebssystem für den Rechner auf dem Exportiert wird empfehle ich Windows Server 2003 oder Windows XP (SP3) in jedem Fall wird aber ein 32 Bit OS benötigt. Außerdem benötigen wir Office (> 2003) ebenfalls in 32 Bit und die Exchange Management Tools (wieder in 32 Bit).

## Abhängigkeiten
Für die Installation der Exchange Management Tools sind einige Vorbereitungen auf dem Rechner zu treffen:

* Microsoft .NET Framework 2.0 Download
* Microsoft Management Console Download
* Microsoft PowerShell 1.0
* Windows Installer 4.5 Download

## Installation

Nachdem wir den ganzen Pakete heruntergeladen und installiert haben können wir mit der Installation der Exchange Management Tools (Download) anfangen. Die Sacher erweist sich als relativ schmerzfrei. Die einzige Besonderheit ist, dass man natürlich (dummer Fehler meinerseits) als Domänenadministrator angemeldet sein muß, da je nach AD Stand noch Schemaänderungen notwendig sind.

Zur Office Installation gibt es weiter auch nichts besonderes zu sagen. Ich habe ausschließlich Outlook und die Gemeinsamen Dateien installiert, das hat ausgereicht. Vergesst nur nicht einmal Outlook zu starten – sonst klappt aus irgendeinem Grund die Verbindung Authorisierung nicht.

## Exportieren der Postfachdaten
Starten wir also die PowerShell (um genau zu sein die Exchange Management Shell) und prüfen ob unser Cmdlet Export-Mailbox zur Verfügung steht

    [PS] C:\> get-help Export-Mailbox
    
    NAME
        Export-Mailbox
    
    [...]

Wenn alles geklappt hat sollte jetzt die Hlife von Export-Mailbox angezeigt werden (und wer will kann Sie auch lesen). Eigentlich können wir uns jetzt an den Export der Daten machen. Allerdings müssen wir, wie oben bereits erwähnt, erst noch für ausreichende Rechte in den Postfächern sorgen.

Geben wir also unserem Administrator Vollzugriff auf alle Postfächer:

    Get-Mailbox | Add-MailboxPermission -User Admin.ADS -AccessRights fullaccess -Inheritance all

Sollten bei diesem Schritt keine Probleme auftauchen können wir uns dem eigentlichen Export zuwenden. Um zu prüfen ob alles korrekt eingerichtet ist exportieren wir zuerst ein einzelnes kleines Postfach:

    Export-Mailbox -identity Beisel -PstFolderPath c:\psts\

Nach einer Sicherheitsabfrage die darauf hinweist, dass dieser Vorgang eventuell viel Zeit in Anspruch nimmt beginnt der Export. Im Anschluss an den Vorgang sollten wir im Ordner c:\PSTs\ die Datei “Beisel.pst” (in meinem Beispiel) vorfinden.

Wenn auch hier wieder keine Probleme aufgetreten sind wenden wir uns nun dem Batch Export zu:

    Get-Mailbox | Export-Mailbox -PstFolderPath c:\psts\

Auch hier erfolgt eine Sicherheitsabfrage (für jedes Postfach) nach deren Bestätigung der Export startet. Je nach Umfang kann dieser Vorgang allerdings sehr viel Zeit in Anspruch nehmen weshalb ich hier vorschlagen würde erstmal Kaffee zu kochen.