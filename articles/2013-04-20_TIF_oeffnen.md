Title: Keine Berechtigung beim Öffnen von TIF Dateien in Outlook
Tags: outlook
 
Microsoft Outlook zeigt beim Öffnen von TIF Dateien an, dass keine Berechtigung besteht.
Die Lösung ist sehr simpel:

[cut]

1. Microsoft Outlook schliessen
2. Registry öffnen (Start --> Ausführen --> Regedit)
3. Zum passenden Eintrag navigieren:

        Outlook 2007:
        HKEY_CURRENT_USER\Software\Microsoft\Office\12.0\Outlook\Security
        Outlook 2010:
        HKEY_CURRENT_USER\Software\Microsoft\Office\14.0\Outlook\Security
        Outlook 2013:
        HKEY_CURRENT_USER\Software\Microsoft\Office\15.0\Outlook\Security

    Im Schlüssel "OutlookSecureTempFolder" ist der Pfad in dem die TIF Dateien und weitere Temp Dateien von Outlook liegen.

4. Den Inhalt des Ordners vom Pfad des Registry Schlüssels löschen.
   Der Inhalt des Ordners kann bedenkenlos komplett gelöscht werden.
