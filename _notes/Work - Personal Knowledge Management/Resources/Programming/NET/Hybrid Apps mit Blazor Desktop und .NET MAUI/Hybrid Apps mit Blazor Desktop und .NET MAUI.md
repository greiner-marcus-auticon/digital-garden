---
title: Hybrid Apps mit Blazor Desktop und .NET MAUI
feed: show
---

[https://www.dev-insider.de/hybrid-apps-mit-blazor-desktop-und-net-maui-a-bfc0c5208b04aa4c56b59e623f4ed349/](https://www.dev-insider.de/hybrid-apps-mit-blazor-desktop-und-net-maui-a-bfc0c5208b04aa4c56b59e623f4ed349/)

Microsoft Blazor ist ein Web-Framework auf Open-Source-Basis. Das Framework kann für hybride Apps genutzt werden, die sowohl Web-Anwendung als auch herkömmliche App sind. In Visual Studio lässt sich das Framework kostenlos nutzen.

[](https://www.notion.sojavascript:void(0))[](https://www.notion.sojavascript:void(0))Blazor-Apps lassen sich zum Beispiel mit Visual Studio 2022 entwickeln.

![[assets/0109614454.jpeg]]

[Microsoft Blazor](https://dotnet.microsoft.com/en-us/apps/aspnet/web-apps/blazor) bietet die Möglichkeit, HTML und CSS bei der Entwicklung von Web-Anwendungen zu verwenden. Hier unterscheidet sich das Open Source-[Framework](https://www.dev-insider.de/was-ist-ein-framework-a-7d1d1ac950ae1456e283bf93ef43ed10/), das unter der Apache-2-Lizenz veröffentlicht wird, nicht von anderen Web-Frameworks. Blazor gibt es in verschiedenen Versionen, Blazor [WebAssembly](https://www.dev-insider.de/was-ist-webassembly-a-4243591d831cc5a12d425ede224f5e5b/)/WASM ist sicherlich eine der bekannteren davon.

Um diese zu erweitern, bietet Microsoft noch Blazor Server. Ebenfalls interessant ist .NET MAUI Blazor, welches auch als Blazor Hybrid bekannt ist. Blazor Desktop ist die vierte Version, die es wiederum ermöglicht, moderne Oberflächen in klassische Anwendungen einzubinden, die auf älteren Windows-Versionen laufen. Natürlich können Windows 10 oder Windows 11 genauso zum Einsatz kommen, wie Server-Versionen ab Windows Server 2008 R2 bis hin zu Windows Server 2022.

Parallel dazu ist es möglich, Programmcode in C# mit der Razor-Syntax zum Einsatz kommen, es ist kein [JavaScript](https://www.dev-insider.de/was-ist-javascript-a-586580/) notwendig. Mit .NET MAUI Blazor lassen sich daher problemlos hybride Anwendungen entwickeln, die auf Android genauso laufen, wie auf Windows oder macOS. Selbst auf Windows 7 lassen sich Blazor-Apps noch nutzen. Die Entwicklung findet mit [Visual Studio](https://www.dev-insider.de/was-ist-visual-studio-a-598e8dd7e88584a9267feb914b69cf12/) 2022 statt.

### C#/.NET für Web und Windows- sowie Smartphone-Apps

Mit Blazor lassen sich daher die Möglichkeiten von C# und .NET in Webanwendungen nutzen. Seinen Ursprung hat Blazor als Web-Framework, das vor allem auf Client-Seite zum Einsatz gekommen ist. Im Fokus stand von Anfang an, dass C# und .NET im Webbrowser nutzbar sein sollten. Die Serverkomponenten von Blazor werden als Razor-Komponenten bezeichnet.

Bei der Entwicklung von neuen Anwendungen, die auf verschiedenen Plattformen laufen sollen, stellen sich vor allem zwei Fragen: Welche [Programmiersprache](https://www.dev-insider.de/was-ist-eine-programmiersprache-a-20b48cdee8d8c3b82221688455e2549e/) soll zum Einsatz kommen? Und: Welches Framework verwenden wir dazu? In der Cross-Plattform-Entwicklung soll eine Anwendung einmal entwickelt werden, aber auf allen gewünschten Plattformen laufen. Blazor kann diese Herausforderungen erfüllen.

[](https://www.notion.sojavascript:void(0))[](https://www.notion.sojavascript:void(0))Starten einer Blazor-App in Visual Studio 2022

![[assets/0109614476.jpeg]]

Blazor-Komponenten können mit jedem Webbrowser mit WebAssembly zum Einsatz kommen. Auf der Serverseite kommt entweder ASP.NET zum Einsatz, oder native Anwendungen. Dadurch lassen sich moderne Oberflächen erstellen und Komponenten aus verschiedenen Apps kombinieren und in verschiedenen Apps nutzen. Modulare Anwendungen können einfach erstellt werden, und zwar deklarativ. Blazor kommt daher vor allem dann zum Einsatz, wenn eine Anwendung auf Desktops, im Web und mobil zum Einsatz kommen soll. Mobile Apps auf Basis von Blazor können dabei auf Android genauso laufen, wie auf iOS.

Blazor Server erweitert Blazor Desktop um weitere Funktionen, mit denen sich zum Beispiel Client-seitige Webanwendungen offline nutzen lassen. Blazor mit WebAssembly/WASM kann nicht auf alle Bibliotheken des .NET-Frameworks zugreifen und dabei auch nicht das Betriebssystem umfassend nutzen.

Der C\#-Code läuft in diesem Fall direkt im Webbrowser auf Basis von WebAssembly. Aus Sicherheitsgründen läuft dieser Code in einer Sandbox, genauso wie JavaScript. Die Kommunikation finden in diesem Fall über die APIs des Browsers oder den Websockets des Servers statt. Alle anderen Zugriffe werden blockiert.

Beim ersten Start ist Blazor WASM etwas langsam. Blazor Server bietet vollständigen Zugriff auf alle .NET-Bibliotheken und alle Funktionen des Betriebssystems. Dazu kommt eine hohe Performance. Hier läuft der C\#-Code direkt auf dem jeweiligen Server und hat die gleichen Möglichkeiten, wie andere .Net-Anwendungen auch.

### Legacy-Anwendungen auf aktuellen Stand heben

[](https://www.notion.sojavascript:void(0))[](https://www.notion.sojavascript:void(0))Blazor kann mit Visual Studio 2022 zum Einsatz kommen.

![[assets/0109614478.jpeg]]

Wer ältere [Legacy](https://www.dev-insider.de/was-ist-legacy-software-a-b7ab39ef381e1caf8ee079a70740e49b/)-Anwendungen [modernisieren](https://www.dev-insider.de/was-ist-software-modernisierung-a-1a03908fed8e97271ace6cc61af3a8e4/) will, kann Blazor sogar in älteren Desktop-Anwendungen auf Basis von Windows Forms oder WPF integrieren. Die Funktionsweise von Blazor Desktop gestaltet sich ähnlich zu Blazor Server. Blazor Desktop/Hybrid kann auf herkömmlichen Windows-Systemen laufen. Dabei besteht vollständigen Zugriff auf die Hardware und das komplette Betriebssystem. Die Anwendungen lassen sich darüber hinaus normal in Windows starten und auch installieren. Allerdings muss dazu mindestens .NET 6, besser neuer zum Einsatz kommen. In Visual Studio 2022 ist es möglich Blazor-Projekte mit .NET 7 zu entwickeln.

### Blazor-basierte Webanwendungen in native Anwendungen einbetten

Entwickeln einer Blazor-App in Visual Studio 2022.

![[assets/0109614477.jpeg]]

Ein beliebtes Einsatzgebiet von Blazor ist wie zu erwarten die Entwicklung von Cross-Plattform-Anwendungen mit einer einheitlichen Code-Basis und möglichst einer identischen GUI. Ein ebenfalls häufiges Einsatzgebiet ist die Integration von vorhandenen Anwendungen mit Webanwendungen, für das gemeinsame Nutzen von Funktionen oder zum Austausch von Daten. Sollen ältere Anwendungen nach und nach modernisiert werden, um zum Beispiel als [Webanwendung](https://www.dev-insider.de/was-ist-eine-web-app-a-596814/) zum Einsatz zu kommen, ist Blazor ebenfalls eine gute Wahl. Auch der umgekehrte Weg ist denkbar, wenn also Webanwendungen als lokale App laufen sollen, weil Zugriff auf Ressourcen notwendig ist, der sonst nicht möglich wäre.

### Mit Blazor entwickeln: Visual Studio 2022 nutzen

Blazor lässt sich einfach mit der kostenlosen Community Edition von Visual Studio 2022 nutzen. Für die [Installation](https://www.dev-insider.de/wie-funktioniert-eine-software-installation-a-609273/) muss „ASP.NET und Webentwicklung“ ausgewählt werden. Beim Erstellen eines neuen Projektes stehen Vorlagen für Blazor-Apps zur Verfügung. Als Framework kann an dieser Stelle zum Beispiel .NET 7.0 ausgewählt werden, es ist aber mindestens .NET 6 notwendig. Danach kann mit der Entwicklung der App fortgefahren werden.

Beim ersten Start installiert Visual Studio ein Zertifikat, das für die Entwicklung von Blazor-Apps genutzt werden kann. Dem Zertifikat sollte vertraut werden. Dazu blendet Windows verschiedene Meldungen ein, die bestätigt werden müssen. Um eine Testseite für das erste Projekt zu erstellen, kann über die Symbolleiste der [Debugger](https://www.dev-insider.de/was-ist-ein-debugger-a-730931/) für die App gestartet werden, dieser startet auch gleich den Webbrowser. Der Inhalt der Beispiel-App wird über die Datei „Index.razor“ definiert, die sich im Verzeichnis „Pages“ befindet. Ihr Inhalt sieht folgendermaßen aus.

Der Debugger lässt sich über das Stop-Symbol jederzeit anhalten. Durch die Beispieldateien ist die Entwicklung mit Blazor keine große Einstiegshürde, vor allem weil auch Visual Studio 2022 Community Edition zum Einsatz kommen kann.
