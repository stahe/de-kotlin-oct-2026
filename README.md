# Kotlin 2.4 lernen

📖 **Das Tutorial lesen: [https://stahe.github.io/de-kotlin-oct-2026/](https://stahe.github.io/de-kotlin-oct-2026/)**

Dieser Kurs vermittelt die Sprache [Kotlin](https://kotlinlang.org/) 2.4 **anhand von Beispielen**: über zweihundert Programme (337 Kotlin-Dateien), zeilenweise kommentiert und mit ihren Ausführungsergebnissen. Er beginnt mit den Grundlagen der Sprache und behandelt Coroutinen, den Datenbankzugriff mit JDBC und Hibernate, die Netzwerkprogrammierung und Webservices mit Spring Boot.

Es ist die Kotlin-Portierung des Kurses [Die Sprache Java 25 lernen](https://stahe.github.io/de-java-oct-2026/) (Oktober 2026), der seinerseits eine Portierung des Kurses [C# 14](https://stahe.github.io/de-csharp-oct-2026/) ist – der Neufassung eines C#-Kurses von 2008: derselbe Aufbau, dieselben Beispiele, derselbe rote Faden, aber in heutigem Kotlin geschrieben. Alle Programme lassen sich mit Kotlin 2.4.20 **ohne Fehler und ohne Warnungen** kompilieren (Warnungen werden als Fehler behandelt) und wurden auf JDK 25 ausgeführt; die im Kurs wiedergegebenen Ergebnisse stammen aus diesen Läufen.

| Java-25-Kurs (2026) | Kotlin-2.4-Kurs (2026) |
|---|---|
| Java 25 (LTS), JDK 25 | Kotlin 2.4.20, JDK 25 |
| kompakte Quelldateien (`java Prog.java`) | `main`-Funktionen auf oberster Ebene, `kotlinc`, das grüne Dreieck von IntelliJ IDEA |
| Maven-Projekte (`pom.xml`) | Gradle-Projekte mit Kotlin DSL (`build.gradle.kts`), Gradle-9.8-Wrapper |
| `null`, Wrapper-Klassen und `Optional` | Null-Sicherheit im Typsystem (`?.`, `?:`, `!!`) |
| Accessoren, Records, keine Operatorüberladung | Eigenschaften, Data Classes, Operatorüberladung, Erweiterungsfunktionen |
| geprüfte Ausnahmen, `try` mit Ressourcen | ausschließlich ungeprüfte Ausnahmen, `use` |
| Stream API | Collection-Operationen, Sequenzen |
| funktionale Schnittstellen, Lambdas | Funktionstypen, Lambdas, `inline`-Funktionen, delegierte Eigenschaften |
| `CompletableFuture`, virtuelle Threads | **Coroutinen**: `suspend`, `launch` / `async`, Structured Concurrency, `Flow`, `Channel` |
| JUnit 6, Spring Framework 7 | JUnit 6 und `kotlin.test`, Spring Framework 7 (Plugin `kotlin-spring`) |
| Jackson 3 | Jackson 3 mit Data Classes |
| JDBC, MySQL Connector/J, HikariCP | dasselbe, mit Kotlin-Idiomen |
| Hibernate 7 | Hibernate 7 mit den Plugins `kotlin-jpa` (No-Arg) und all-open |
| `HttpClient`, `Socket` / `ServerSocket` | dieselben Klassen, in Kotlin |
| Spring Boot 4 | Spring Boot 4 in Kotlin |

## Kursgliederung

| Kapitel | Inhalt |
|---|---|
| Installation | JDK 25, IntelliJ IDEA / VS Code, der Compiler `kotlinc`, Gradle (Wrapper, Kotlin DSL), Multimodul-Projekte |
| Grundlagen der Sprache | Typen, `val` / `var`, Typinferenz, String-Templates, Raw Strings, Null-Sicherheit, Konvertierungen, Arrays, Operatoren, `when`, Bereiche und Schleifen, Ausnahmen, `use`, Enums, Standard- und benannte Parameter, lokale Funktionen, `Pair` und Destrukturierung |
| Klassen, Schnittstellen, Data Classes | primäre Konstruktoren, Eigenschaften, `object` und `companion object`, Vererbung, Sealed Classes, Operatorüberladung, Indexer, Value Classes, Erweiterungsfunktionen, Schnittstellen, abstrakte Klassen, Generics und Varianz, Pakete, Data Classes, intelligente Typumwandlung (Smart Casts) |
| Häufig verwendete Kotlin-Klassen | Zeichenketten, Arrays, schreibgeschützte und veränderbare Collections, funktionale Operationen, **Sequenzen**, Text- und Binärdateien, JSON mit Jackson 3, reguläre Ausdrücke (`Regex`) |
| Schichtenarchitekturen | [DAO] / [Geschäftslogik] / [UI], Gradle-Builds mit mehreren Projekten, Unit-Tests mit **JUnit 6** und `kotlin.test`, **Dependency Injection** mit Spring |
| Funktionstypen, Lambdas und Ereignisse | Funktionstypen, Lambdas, Funktionsreferenzen, `fun interface`, Closures, Komposition, `inline`-Funktionen, Listener, beobachtbare delegierte Eigenschaften |
| Ausführungs-Threads | `thread { }`, **virtuelle Threads**, `synchronized`, `@Volatile`, `ReentrantLock`, `Condition`, atomare Operationen, Semaphoren, nebenläufige Collections, `ExecutorService`, Fork/Join, `ThreadLocal`, `ScopedValue` |
| Asynchrone Programmierung | **Coroutinen**: `suspend`-Funktionen, `runBlocking`, `launch`, `async` / `await`, Dispatcher, Structured Concurrency, Ausnahmen, Abbruch und Timeouts, `Flow`, `Channel`, `CompletableDeferred`, begrenzte Parallelität, Coroutinen und virtuelle Threads |
| Datenbankzugriff mit JDBC | MySQL, MySQL Connector/J, `DataSource` und HikariCP, parametrisierte Abfragen und SQL-Injection, Transaktionen, Batches, JDBC mit virtuellen Threads und Coroutinen |
| **Hibernate** | Kotlin-Entitäten, `SessionFactory` / `Session`, CRUD, HQL und Criteria API, Beziehungen, Migrationen, optimistische Nebenläufigkeit, native SQL, Spring ORM und `@Transactional` |
| Webprogrammierung | TCP/IP, IPv6, TCP-Clients und -Server (ein virtueller Thread pro Client), HTTP, `HttpClient`, SMTP mit Jakarta Mail |
| Webservices | REST / JSON mit **Spring Boot 4** in Kotlin, Kotlin-Konsolenclients, JavaScript-Webclient, Node.js-Client |

## Der rote Faden: ein Steuerrechner in 9 Versionen

Im Verlauf des Kurses bauen wir Version für Version eine Anwendung zur **Berechnung der Einkommensteuer** auf:

- **Version 1**: ein einziges Programm; **Version 2**: Klassen und Schnittstellen; **Version 3**: Daten, die aus einer Text- oder JSON-Datei gelesen werden;
- **Versionen 4 und 5**: eine Schichtenarchitektur, gegliedert in Gradle-Teilprojekte, getestet mit JUnit und per Dependency Injection mit Spring integriert;
- **Versionen 6 und 7**: in einer MySQL-Datenbank gespeicherte Daten, die zunächst mit JDBC und dann mit Hibernate abgerufen werden;
- **Version 8**: ein TCP-Server zur Steuerberechnung und sein Client;
- **Version 9**: ein REST-Webservice mit Spring Boot, sein Konsolenclient und sein Webclient.

## Technologien

Kotlin 2.4.20 · JDK 25 · Gradle 9.8 (Kotlin DSL) · IntelliJ IDEA · kotlinx.coroutines 1.11 · JUnit 6 · Spring Framework 7 · Jackson 3 · MySQL 8 · MySQL Connector/J 9 · HikariCP · Hibernate ORM 7.4 · Jakarta Persistence 3.2 · Spring Boot 4.1 · Laragon

## Voraussetzungen

- Vorherige Programmiererfahrung in einer beliebigen Sprache.
- Ein [JDK 25](https://adoptium.net) (Eclipse Temurin oder eine andere Distribution) und [IntelliJ IDEA](https://www.jetbrains.com/idea/) (Kotlin ist integriert). Gradle muss nicht installiert werden: Jeder Kapitelordner enthält seinen eigenen Wrapper (`gradlew`). Ist JDK 25 nicht installiert, lädt Gradle es automatisch herunter (über das Plugin `foojay-resolver-convention`). [Laragon](https://laragon.org) für MySQL (Kapitel zu Datenbanken). Anleitungen zur Installation finden sich im Kurs.

## Autor

Dieser Kurs und sein Code wurden von **Claude**, der KI von [Anthropic](https://www.anthropic.com) (Oktober 2026), im Auftrag von Serge Tahé geschrieben, auf der Grundlage seines Java-25-Kurses.

Prüfer: [Serge Tahé](https://stahe.github.io)
