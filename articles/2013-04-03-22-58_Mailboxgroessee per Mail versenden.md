Title: Exchange Mailboxgrößen per Mail versenden
Tags: exchange

Dieser Artikel ist in ähnlicher Form vor 2 Jahren auf [pacey.me](http://pacey.me) veröffentlicht.

In meinem letzten Beitrag wollte ich beschreiben wie man mithilfe der Powershell die Mailboxgrößen auf einem Exchange 2007 Server abfragen kann. In diesem Beitrag stelle ich nun ein Script von mir vor welches automatisch aufgerufen, dazu eingesetzt werden kann regelmäßige Statistiken der Mailboxgrößen zu erstellen und per Mail zu versenden. 

Ein möglicher Einsatzbereich, wäre in meinen Augen die Auswertung interner Mailkonten gerade in Bereichen, in denen Große Datenmengen per E-Mail versendet werden müssen (z.B.: das Marketing bzw. Druckereien)

    ###Send mailbox statistics script
    
    ###First, the admin must change the mail message values in this section
    $FromAddress = "MailboxReport@domain.local"
    $ToAddress = "mail@notset.com"
    
    $MessageSubject = "Mailbox Grössen Report"
    $MessageBody = "Im Anhang befindet sich die aktualle Liste mit den Mailboxgrößen"
    $SendingServer = "mail.example.com"
    
    ### Fetch Statistics and Store in a LogFile. 
    Get-MailboxStatistics | where {$_.ObjectClass –eq “Mailbox”} 
                          | Sort-Object TotalItemSize –Descending 
                          | ft DisplayName,ItemCount,@{ label="TotalItemSize(MB)";
                            expression={$_.TotalItemSize.Value.ToMB()}} > C:\Temp\mailboxes.txt
    
    ### Create an e-mail message with the statistics file attached
    $SMTPMessage = New-Object System.Net.Mail.MailMessage $FromAddress, $ToAddress,
    $MessageSubject, $MessageBody
    $Attachment = New-Object Net.Mail.Attachment("C:\Temp\mailboxes.txt")
    $SMTPMessage.Attachments.Add($Attachment)
    
    ###Send the message
    $SMTPClient = New-Object System.Net.Mail.SMTPClient $SendingServer
    $SMTPClient.Send($SMTPMessage)
