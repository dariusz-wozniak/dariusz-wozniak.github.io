---
title: Kurs TDD cz. 2 — Testy jednostkowe, a testy integracyjne
date: "2013-05-28T21:19:04.000Z"
layout: post
draft: false
path: "/posts/kurs-tdd-2-testy-jednostkowe-a-testy-integracyjne/"
category: "Programowanie"
tags:
  - "TDD"
  - "Kurs TDD"
description: "W części drugiej kursu przyjrzymy się rodzajom testów w świecie Test-Driven Development i poznamy różnice między testami jednostkowymi, a integracyjnymi."
---

W [części pierwszej](http://dariuszwozniak.net/2013/04/20/kurs-tdd-czesc-1-wstep/ "części pierwszej") kursu omówiłem czemu służy technika i jak wygląda proces pisania wedle TDD, opisałem cykl Red-Green-Refactor, a na koniec przedstawiłem korzyści płynące z jej używania. W części drugiej kursu przyjrzymy się rodzajom testów w świecie Test-Driven Development i poznamy różnice między testami jednostkowymi, a integracyjnymi. Cztery główne rodzaje testów w świecie TDD to:

*   testy jednostkowe (_unit tests_) — testujemy pojedynczą, jednostkową część kodu: zazwyczaj klasę lub metodę;
*   testy integracyjne (_integration tests_) — testujemy kilka komponentów systemu jednocześnie;
*   testy regresyjne (_regression tests_) — po wprowadzeniu naszej zmiany uruchamiane są wszystkie testy w danej domenie biznesowej celem sprawdzenia czy zmiana nie spowodowała błędu w innej części systemu;
*   testy akceptacyjne (_acceptance tests_) — testy mające na celu odpowiedzieć na pytanie czy aplikacja spełnia wymagania biznesowe.

Jest tego sporo więcej, testy black/white box, testy zgodności (_conformance tests)_, _smoke tests_, itp. W TDD kluczową rzeczą jest zrozumienie różnic między testami jednostkowymi, a integracyjnymi.

# Różnice m. testami jednostkowymi, a integracyjnymi

Aby zrozumieć TDD należy wiedzieć czym jest test jednostkowy oraz czym jest integracyjny. TDD oparte jest tylko i wyłącznie o testy jednostkowe, tj.—piszemy jednostkowe testy przed implementacją kodu (Red-Green-Refactor), badamy pokrycie testów jednostkowych, uruchamiamy przy domyślnym buildzie testy jednostkowe. Testy integracyjne mogą być pisane opcjonalnie. Podstawową różnicą między obydwoma rodzajami testów jest to, że **testy jednostkowe testują pojedyncza część kodu**, natomiast **testy integracyjne mają na celu sprawdzenie kilka komponentów działających razem**. W teście jednostkowym wszystkie zależności powinny być zastąpione przez tzw. _mock objects_ (będzie o tym mowa w kolejnych częściach), które symulują ich zachowanie. W teście integracyjnym możemy skorzystać z kilku lub wszystkich zależności systemu. Dla przykładu—funkcjonalność naszej metody to pobranie z bazy danych pewnych informacji i zwrócenie posortowanych danych. Uszczegółówmy sobie ten przykład jeszcze bardziej—pobieramy z bazy danych listę osób i sortujemy je alfabetycznie wg nazwiska. W testach jednostkowych musimy zasymulować (albo: zastąpić) połączenie z bazą danych i przygotować ręcznie listę osób. Możemy to najprościej wykonać poprzez wspomniane już _mock objects_, które mogą być stworzone ręcznie lub przy pomocy dowolnego frameworka do mockowania. W teście integracyjnym możemy sobie pozwolić na połączenie z rzeczywistą bazą danych, dzięki czemu będziemy operować na rzeczywistych danych, pobranych z bazy. Drogi Czytelniku! Jeśli powyższy akapit nie jest dla Ciebie do końca jasny, nie przejmuj się! Powinieneś wiedzieć jedynie, że w testach jednostkowych klasy powinny być w izolacji względem innych zależności, tj. innych klas i zasobów (np. baza danych, Internet, input/output). Wszystkie te zależności powinny być _mockowane_. Jest to jednak temat na dalszą część tego kursu. Jeśli nauczysz się jak _mockować_ obiekty, wszystko stanie się o wiele jaśniejsze :) Ta i pozostałe różnice zostały podane w poniższej tabeli:

Zagadnienie

Test jednostkowy

Test integracyjny

**Zależności**

Testowany **jednostkowy element (klasa, metoda) w izolacji**.

Testowana **więcej niż jedna wewnętrzna lub zewnętrzna zależność.**

**Punkt awarii (_failure point_)**

Tylko **jeden potencjalny punkt awarii** (jedna logiczna asercja per test*).

**Wiele potencjalnych punktów awarii.**

**Szybkość działania**

**Bardzo szybko**, dużo poniżej 1 sekundy.

**Może trwać długo**, ze względu na czasochłonne operacje np. dostęp do bazy danych, I/O, operacje na sesji.

**Konfiguracja**

Test musi działać na każdej maszynie **bez dodatkowej konfiguracji.**

Test **może być zależny od konfiguracji**, np. machine.config (login/hasło) do bazy danych.

\* — Test jednostkowy może zawierać więcej niż jeden Assert pod warunkiem, że wszystkie testują jeden element. Czytaj więcej: [http://ayende.com/blog/2684/what-is-one-assert](http://ayende.com/blog/2684/what-is-one-assert "http://ayende.com/blog/2684/what-is-one-assert").

# Struktura w kodzie

W związku z różnicami jakie dzielą obydwa typy testów, zarówno jednostkowe, jak i integracyjne testy powinny być umieszczone w oddzielnych projektach: [![solution](http://dariuszwozniaknet.files.wordpress.com/2013/05/solution.png)](ed3218cd-f05e-4116-a534-d13e772c971a.png)

# Bonus!

Dobrą ilustracją testu jednostkowego i testu integracyjnego są poniższe zdjęcia: [![UnitTest](814b2a81-0f4c-4da0-8727-9cdb0940c194.jpg "UnitTest")](http://dariuszwozniaknet.files.wordpress.com/2013/05/carparts.jpg) W teście jednostkowym testujemy nasze atomiczne elementy (klasy, metody) w izolacji od pozostałych elementów systemu.  
Źródło zdjęcia: [http://www.rpmgo.com/automotive-articles/car-parts.html](http://www.rpmgo.com/automotive-articles/car-parts.html "http://www.rpmgo.com/automotive-articles/car-parts.html") [![IntegrationTest](addc32e6-a4f9-48d6-83bb-7475e39ac6b0.jpg "IntegrationTest")](http://dariuszwozniaknet.files.wordpress.com/2013/05/crashtest.jpg) W teście integracyjnym testujemy wiele lub wszystkie moduły naszego systemu.  
Źródło zdjęcia: [http://indianautosblog.com/2008/12/iihs-suzuki-sx4-crash-test](http://indianautosblog.com/2008/12/iihs-suzuki-sx4-crash-test "http://indianautosblog.com/2008/12/iihs-suzuki-sx4-crash-test")