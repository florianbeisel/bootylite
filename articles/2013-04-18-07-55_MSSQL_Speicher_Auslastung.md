Title: Speichernutzung des Microsoft SQL Servers anpassen
Tags: mssql, speicher

In den letzten Tagen habe ich die 2 physikalische SQL Server Systeme zu Testzwecken virtualisiert. 
Da ich selbst erstmal suchen mußte wie ich nach der Konvertierung den SQL Server auf ein (für VMs) 
vernünftiges Maß an Arbeitsspeicher einstelle wollte ich gerne jemand anderem (oder mir in einem Jahr) 
die Sucherei ersparen:

Um die Speicherauslastung des SQL Servers zu konfigurieren müssen wir im Advanced Mode sein

    EXEC sp_configure 'show advanced options', 1 RECONFIGURE WITH OVERRIDE
 
Die eigentliche Änderung des zugewiesenen Arbeitsspeichers erfolgt mit diesem Befehl:

    EXEC sp_configure 'max server memory (MB)', 512 RECONFIGURE WITH OVERRIDE

Zu beachten ist die Direktive RECONFIGURE WITH OVERRIDE die dafür sorgt, dass die Änderungen sofort
und nicht erst nach Neustart des Dienstes erfolgen.
