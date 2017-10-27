---
TOCTitle: Implementieren der Sperrung
Title: Implementieren der Sperrung
ms:assetid: '4735f060-7197-4ae2-830a-f91bcc4de30a'
ms:contentKeyID: 18118823
ms:mtpsurl: 'https://technet.microsoft.com/de-de/library/Cc747554(v=WS.10)'
---

Implementieren der Sperrung
===========================

Zertifikate und Lizenzen können von einem beliebigen Prinzipal gesperrt werden, der der Vertrauenskette des Zertifikats oder der Lizenz angehört. Von einem Stammzertifizierungsserver ausgestellte Zertifikate oder Lizenzen können von diesem Stammzertifizierungsserver gesperrt werden. Darüber hinaus können die Zertifikate von einem vom RMS-Administrator (Rights Management Services oder Dienste für die Rechteverwaltung) ausgewählten Drittanbieter gesperrt werden.

Sie können eine Sperrliste erstellen und verteilen und dann in einer Vorlage für Benutzerrechterichtlinien festlegen, dass die Liste erforderlich ist, um vom Server mit RMS erteilte Zertifikate oder Lizenzen zu sperren. Dies wird in den folgenden Schritten beschrieben:

1.  Erstellen Sie eine Sperrliste, die die zu sperrenden Prinzipale angibt. Hierbei handelt es sich um eine einfache Textdatei im XML-Format, die dem XrML-Vokabular entspricht. Weitere Informationen finden Sie nachstehend unter „[Erstellen von Sperrlisten](https://technet.microsoft.com/1ef75199-3344-4225-84de-a863a777696a)“.
2.  Generieren Sie ein Schlüsselpaar für die Sperrliste. Signieren Sie die Datei dann mithilfe des zu diesem Zweck bereitgestellten Tools zum Signieren von Sperrlisten mit dem privaten Schlüssel. Anweisungen dazu finden Sie nachstehend unter „Einfügen einer Signatur in eine Sperrliste“. Damit dieser Prozess möglichst regelmäßig abläuft (vorzugsweise täglich), sollten Sie ihn automatisieren.
3.  Legen Sie die Sperrliste an einem Speicherort ab, auf den alle Benutzer Zugriff haben, die sie benötigen. Die Datei sollte an einem Speicherort abgelegt werden, auf den sowohl vom Netzwerk als auch vom Internet aus zugegriffen werden kann, vorzugsweise auf einer Website. Damit wird sichergestellt, dass Benutzer sowohl im Firmennetzwerk als auch von außerhalb auf die Datei zugreifen können.
4.  Erstellen Sie eine Vorlage für Benutzerrechterichtlinien, die eine Anforderung für die Sperrliste einschließt. Weitere Informationen hierzu finden Sie nachstehend unter [Erstellen und Ändern von Vorlagen für Benutzerrechterichtlinien](https://technet.microsoft.com/6014176f-ef71-4d29-b3e3-da129c18563d).

Sie können außerdem das Server-Lizenzgeberzertifikat des Stammzertifizierungsservers sperren. Da dieses Zertifikat vom Microsoft-Registrierungsdienst ausgestellt wurde, kann Microsoft das Server-Lizenzgeberzertifikat der Stammzertifizierung sperren. Dazu fügt Microsoft das Server-Lizenzgeberzertifikat zu seiner Sperrliste hinzu und stellt diese Liste öffentlich zur Verfügung.

Darüber hinaus kann das Server-Lizenzgeberzertifikat des Stammzertifizierungsservers durch einen Drittanbieter gesperrt werden, wenn der RMS-Administrator diese Option während der Bereitstellung aktiviert hat. Wenn Sie diese Option verwenden, sollte eine Sperrliste, die das vom privaten Schlüssel des Drittanbieters signierte Server-Lizenzgeberzertifikat enthält, für Clients zur Verfügung gestellt werden. Weitere Informationen finden Sie nachstehend unter „[Sperren von Server-Lizenzgeberzertifikaten](https://technet.microsoft.com/8020861d-d196-4431-8282-044675ef5616)“.

| ![](images/Cc747554.Caution(WS.10).gif)Vorsicht                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gehen Sie beim Implementieren der Sperrung mit besonderer Sorgfalt vor. Auf der Basis des angegebenen Aktualisierungsintervalls müssen Sie eine Sperrliste regelmäßig erneuern. Andernfalls läuft deren Gültigkeit automatisch ab, so dass Benutzer Inhalte nicht mehr abrufen können, für die diese Liste erforderlich ist. Werten Sie das erforderliche Intervall zum Aktualisieren der Sperrliste sorgfältig aus, um sicherzustellen, dass Benutzer nicht versehentlich am Abrufen von Inhalten gehindert werden. Stellen Sie außerdem sicher, dass der Zugriff auf die Sperrliste im Netzwerk und von außerhalb möglich ist. Weitere Informationen finden Sie oben unter „[Definieren von Sperrrichtlinien](https://technet.microsoft.com/e2fffe9f-def7-439b-a8aa-43f8a065813d)“. |