---
TOCTitle: Gesamtstrukturübergreifendes Bereitstellen von RMS
Title: Gesamtstrukturübergreifendes Bereitstellen von RMS
ms:assetid: 'd531dfdc-efff-4eb0-8d99-f1fd19d7a963'
ms:contentKeyID: 18119038
ms:mtpsurl: 'https://technet.microsoft.com/de-de/library/Cc747685(v=WS.10)'
---

Gesamtstrukturübergreifendes Bereitstellen von RMS
==================================================

Wenn in Ihrer Organisation mehrere Active Directory-Gesamtstrukturen eingesetzt werden und Sie RMS unternehmensweit verwenden möchten, sollten Sie sicherstellen, dass Sie das Netzwerk so konfiguriert haben, dass RMS die in den verschiedenen Gesamtstrukturen befindlichen Benutzerkonten und Verteilungsgruppen zuordnen und authentifizieren kann.

RMS verwendet Active Directory zur Zuordnung von Benutzern und Verteilergruppen. Wenn die Bereitstellung von Active Directory in einer Organisation mehrere Gesamtstrukturen umfasst, verwendet RMS Kontaktobjekte zum Abrufen der Benutzer- und Gruppenidentitäten, die einer anderen Gesamtstruktur als dem RMS-Server angehören. Dazu sind drei Voraussetzungen erforderlich:

1.  Die RMS-Zertifizierungsserver müssen sich in einer anderen Gesamtstruktur befinden, und die Kontaktobjekte müssen für die einzelnen Remotegruppen definiert werden.
2.  In Gesamtstrukturen, die Kontaktobjekte enthalten, müssen Schemaerweiterungen bestehen, mit deren Hilfe sie auf die Gesamtstrukturen mit den tatsächlichen Objekten zurückverweisen können.
3.  Attribute in den Kontaktobjekten müssen synchronauf die Gesamtstrukturen mit dem tatsächlichen Objekt zurückverweisen.

Dies wäre beispielsweise der Fall, wenn die Festlegung und Verwaltung der Gruppen in einer Gesamtstruktur, die Festlegung und Verwaltung der Benutzer jedoch in einer anderen Gesamtstruktur erfolgt. Ein Benutzer möchte Rechte an Inhalten zuweisen, die auf der Mitgliedschaft an einer bestimmten Gruppe basieren. In diesem Szenario ist der *RMS\_Server\_B* der Server in der Gesamtstruktur, der die Benutzerkonten enthält, und *RMS\_Server\_G* der Server in der Gesamtstruktur mit den Gruppenkonten. Eine vertrauenswürdige Benutzerdomäne wurde zwischen diesen zwei RMS-Servern erstellt. Damit *RMS\_Server\_B* die in der Veröffentlichungslizenz festgelegte Gruppenmitgliedschaft erfolgreich gesamtstrukturübergreifend erweitern kann, muss in der lokalen Active Directory-Gesamtstruktur ein Kontaktobjekt vorhanden sein, dass die Gruppe in der entfernten Gesamtstruktur repräsentiert. RMS kann die Attribute des Kontaktobjekts abfragen und feststellen, dass das Objekt eine Gruppe aus einer anderen Gesamtstruktur repräsentiert. Danach kann RMS einen RMS-Server in dieser Gesamtstruktur suchen und ermitteln, ob zwischen diesem und dem anderen RMS-Server eine Vertrauensstellung besteht. Wenn *RMS\_Server\_B* das Active Directory nach der in der Veröffentlichungslizenz angegebenen Gruppe abfragt, wird durch das Kontaktobjekt klar, dass die Gruppe einer anderen Gesamtstruktur angehört. Danach leitet *RMS\_Server\_B* die Anforderung an den RMS im Dienstverbindungspunkt (Service Connection Point oder SCP) für diese Domäne weiter. Der SCP ordnet *RMS\_Server\_G* als RMS-Zertifizierungsserver für die Domäne zu. *RMS\_Server\_B* fragt danach *RMS\_Server\_G* hinsichtlich der Mitgliedschaft dieser Gruppe ab.

Das von RMS nach diesen Informationen abgefragte Attribut lautet **msExchOriginatingForest**. Dieses Attribut wird standardmäßig im Active Directory-Schema installiert, wenn Sie über eine Installation von Exchange Server 2003 in der Gesamtstruktur verfügen. Das Attribut muss in der Gesamtstruktur jedes Active Directory-Schemas vorhanden sein, das an RMS teilnimmt. Wenn Sie Exchange Server 2003 nicht verwenden, können Sie diese Schemaerweiterungen hinzufügen, indem Sie das RMS-Verwaltungs-Toolkit herunterladen. Dies umfasst eine Schemadatei und Anweisungen dazu, wie die Datei zu Active Directory hinzuzufügen ist.

Diese Attribute müssen synchronisiert werden, damit das Kontaktobjekt auf das tatsächliche Objekt in anderen Gesamtstrukturen verweist. Nach dem Hinzufügen des Attributs zum Active Directory-Schema müssen Sie den Attributwert als voll gekennzeichneten Domänennamen (Fully Qualified Domain Name oder FQDN) der Gesamtstruktur, in der sich die Gruppe befindet, konfigurieren, z. B. corp.fabrikam.com.

Dies ist mit Microsoft Identity Integration Server (MIIS) 2003 oder Identity Integration Feature Pack (IIFP) sowie dem Verwaltungsagent für die globale Adressenliste (GAL) von Active Directory möglich.

Sie müssen den lokalen RMS-Server mit ausreichenden Berechtigungen ausstatten, damit er das entfernte Active Directory durchsuchen und die .NET Remoting-Schnittstellen, die sich auf der entfernten RMS-Installation befinden, aufrufen kann.

Zur Unterstützung der gesamtstrukturübergreifenden Gruppenerweiterung muss das für das RMS-Dienstkonto in den einzelnen Gesamtstrukturen verwendete Konto auch Lese- und Ausführungsberechtigungen für Active Directory besitzen. RMS gewährt allen authentifizierten Benutzern mit Domäneninformationen automatischen Lesezugriff auf Active Directory. Zur Erhöhung der Sicherheit können Sie diesen Zugriff aus der freigegebenen Zugriffssteuerungslisten (Discretionary Access Control Lists oder DACL) entfernen und ihn durch individuelle Dienstkonten aus den verschiedenen Gesamtstrukturen ersetzen.

Die für das RMS-Dienstkonto erforderlichen Berechtigungen sind von den vorhandenen Active Directory-Vertrauensstellungen zwischen der lokalen Gesamtstruktur und den Remotegesamtstrukturen abhängig. In der folgenden Liste werden die Vertrauensmodelle und die erforderlichen Berechtigungen identifiziert.

-   **Die Vertrauensstellung besteht in zwei Richtungen**. Die lokale Gesamtstruktur mit RMS vertraut der Gesamtstruktur, aus der die Gruppe stammt, und wird von dieser als vertrauenswürdig angesehen. Beim RMS-Dienstkonto für den RMS-Server in einer bestimmten Gesamtstruktur kann es sich um ein beliebiges gültiges Domänenkonto innerhalb dieser Gesamtstruktur handeln. Stellen Sie sicher, dass Sie das lokale RMS-Dienstkonto zur Zugriffssteuerungsliste des Ordners \\Inetpub\\wwwroot\\\_wmcs\\drmRemote auf allen RMS-Servern innerhalb der Ausgangsgruppe der Gesamtstruktur hinzufügen.
-   **Es besteht ein Vertrauensstellung in eine Richtung**. Die lokale RMS-Gesamtstruktur vertraut der Gesamtstruktur, aus der die Gruppe stammt, wird von dieser aber nicht als vertrauenswürdig angesehen. Beim RMS-Dienstkonto für alle RMS-Server innerhalb der Organisation muss es sich um ein gültiges Domänenkonto in einer vertrauenswürdigen Gesamtstruktur handeln. Fügen Sie dieses Konto zur Zugriffssteuerungsliste des Ordners \\Inetpub\\wwwroot\\\_wmcs\\drmRemote auf allen RMS-Servern hinzu, die sich in der Gesamtstruktur befinden, aus der die Gruppe stammt.
-   **Es gibt keine Vertrauensstellungen**. Die Gesamtstrukturen in der Organisation können keine Benutzer und Gruppen aus anderen Gesamtstrukturen authentifizieren. Sie sollten keine Gruppenerweiterungen über Gesamtstrukturen hinweg einsetzen, wenn die Gesamtstrukturen keine Vertrauensstellungen aufweisen. Falls dies jedoch eine betriebliche Anforderung darstellt, können Sie dieses Szenario ermöglichen, indem Sie das RMS-Dienstkonto als gültiges Domänenkonto in beiden Gesamtstrukturen konfigurieren und denselben Benutzernamen und dasselbe Kennwort für beide Konten verwenden. Darüber hinaus muss das Konto des lokalen Computers auf allen RMS-Frontend-Servern mit genau denselben Benutzernamen und Kennwörtern wie die für das RMS-Dienstkonto in beiden Gesamtstrukturen verwendeten Domänenkonten erstellt werden. Dadurch erhält der lokale Dienst automatisch genügend Berechtigungen zur Authentifizierung des entfernten Active Directory und des entfernten RMS-Servers.

Verwenden von RMS-Vertrauensrichtlinien
---------------------------------------

Bei der Bereitstellung von RMS in einer Organisation mit mehreren Gesamtstrukturen muss in jeder Gesamtstruktur, die an dem RMS-System teilnehmende Benutzerkonten hostet, ein RMS-Zertifizierungsserver bereitgestellt werden. Wenn die Benutzer der verschiedenen Gesamtstrukturen in der Lage sein sollen, den RMS-geschützten Inhalt gemeinsam zu nutzen, müssen Sie die RMS-Vertrauensrichtlinien so konfigurieren, dass den vom anderen RMS-Server erstellten Zertifikaten und Lizenzen vertraut werden kann. Es gibt zwei Vertrauensrichtlinien, die mit RMS verwendet werden können: vertrauenswürdige Benutzerdomänen und vertrauenswürdige Veröffentlichungsdomänen. Mithilfe von vertrauenswürdigen Benutzerdomänen kann ein RMS-Server den von einem anderen RMS-Zertifizierungsserver erstellten Rechtekontozertifikaten (Rights Account Certificates oder RACs) vertrauen und Nutzungslizenzen an Benutzer mit RACs vom anderen Server ausstellen. Anhand von Veröffentlichungsdomänen kann ein RMS-Server Nutzungslizenzen auf Grundlage von Veröffentlichungslizenzen erstellen, die den anderen Lizenzierungsserver angeben.

Im Folgenden sind einige Optionen zum Einsatz von Vertrauensrichtlinien zur Unterstützung mehrerer Gesamtstrukturen angeführt:

-   In jeder Gesamtstruktur liegt ein RMS-Zertifizierungscluster mit einem einzigen Lizenzierungscluster vor, der von allen Benutzern gemeinsam genutzt wird. Der RMS-Lizenzierungscluster ist mit einer vertrauenswürdigen Benutzerdomäne konfiguriert, die alle RMS-Zertifizierungscluster enthält. Die RMS-Clients sind unter Einsatz eines Registrierungsschlüssels so konfiguriert, dass Sie zum Erwerben von Nutzungslizenzen eine Verbindung mit dem Lizenzierungscluster herstellen.
-   Ein RMS-Zertifizierungs- und Lizenzierungscluster innerhalb einer Gesamtstruktur mit einer vertrauenswürdigen Benutzerdomäne, die auf jedem Cluster so konfiguriert ist, dass den RMS-Servern in den anderen Gesamtstrukturen vertraut wird. Die einzelnen Benutzer verwenden den RMS-Server ihrer Gesamtstruktur zum Erwerben von Nutzungslizenzen und können ihren Lizenzierungsserver über den Dienstverbindungspunkt in Active Directory suchen.
-   Wenn die Benutzer von RMS-geschütztem Inhalt einer Gesamtstruktur angehören, die keinen Zugriff auf jene Gesamtstruktur hat, aus der der veröffentlichte Inhalt stammt, dann kann eine vertrauenswürdige Veröffentlichungsdomäne erstellt werden, damit der Inhalt lizenziert und gelesen werden kann. Vertrauenswürdige Veröffentlichungsdomänen erfordern den privaten Schlüssel des RMS-Servers, der für die Veröffentlichung des zu importierenden Inhalts verwendet wurde.

Weitere Informationen über das Konfigurieren von Vertrauensrichtlinien finden Sie unter „Verwalten von Vertrauensstellungen und Vertrauensrichtlinien“ im Abschnitt „Betreiben eines RMS-Servers“ dieser Dokumentationssammlung.