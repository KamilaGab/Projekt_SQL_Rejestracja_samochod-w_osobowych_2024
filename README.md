# Projekt_SQL_Rejestracja_samochod-w_osobowych_2024

Analiza Sprzedaży Samochodów w Polsce 2024
Opis projektu
Projekt zawiera kompleksową analizę sprzedaży samochodów osobowych w Polsce w 2024 roku, opartą na danych z portalu dane.gov.pl. Baza danych MySQL została zaprojektowana do przechowywania i analizy danych sprzedażowych z podziałem na miesiące i województwa.
Struktura bazy danych
Tabele główne:

kategoria - kategorie pojazdów (osobowe, dostawcze, ciężarowe)
samochody - informacje o markach, modelach i spalaniu
województwa - słownik województw
miesiące - słownik miesięcy
sprzedaz_[miesiac]2024 - dane sprzedażowe dla każdego miesiąca (styczeń-listopad)

Relacje:

Tabela samochody powiązana z kategoria (klucz obcy)
Tabele sprzedaży powiązane z województwa (klucz obcy)

Funkcjonalności
1. Zarządzanie danymi podstawowymi

Tworzenie i zarządzanie kategoriami pojazdów
Dodawanie informacji o samochodach (marka, model, spalanie)
Zarządzanie słownikami województw i miesięcy

2. Analiza sprzedaży

Analiza sprzedaży według województw
Porównanie sprzedaży między miesiącami
Analiza konkretnych marek (szczególnie Mazda)
Statystyki min/max sprzedaży

3. Zaawansowane zapytania

Łączenie danych z wielu miesięcy
Grupowanie i agregacje
Sortowanie wyników
Warunki logiczne z HAVING

Przykłady użycia
Analiza sprzedaży Mazdy w województwach:
sqlSELECT WOJEWODZTWO, MARKA, SUM(LICZBA) as sprzedaz_styczen
FROM sprzedaz_styczen2024
WHERE MARKA = 'MAZDA'
GROUP BY WOJEWODZTWO
ORDER BY sprzedaz_styczen DESC;
Zestawienie roczne sprzedaży:
sqlSELECT s1.WOJEWODZTWO, s1.MARKA, s1.sprzedaz_styczen, s2.sprzedaz_luty, ...
FROM (...) s1
LEFT JOIN (...) s2 ON s1.WOJEWODZTWO = s2.WOJEWODZTWO
...
Wymagania techniczne

MySQL 8.0 lub nowszy
MySQL Workbench (opcjonalnie)
Kodowanie UTF-8

Instalacja i uruchomienie

Klonowanie repozytorium:

bashgit clone [url-repozytorium]
cd analiza-sprzedazy-samochodow

Utworzenie bazy danych:

sqlCREATE SCHEMA IF NOT EXISTS `zaliczenie` DEFAULT CHARACTER SET utf8;
USE `zaliczenie`;

Uruchomienie skryptu:


Zaimportuj plik SQL do MySQL
Uruchom skrypt w MySQL Workbench lub przez wiersz poleceń

Źródła danych
Dane pochodzą z oficjalnego portalu danych otwartych Rzeczypospolitej Polskiej:

Marki samochodów osobowych w 2024 roku

Struktura plików
├── paste.txt                 # Główny skrypt SQL
├── README.md                 # Dokumentacja projektu
└── dane/                     # Folder z danymi źródłowymi (jeśli dostępne)


Najważniejsze funkcje
1. Czyszczenie danych
Skrypt zawiera procedury czyszczenia błędnych nazw województw:
sqlUPDATE sprzedaz_luty2024 SET WOJEWODZTWO='DOLNOŚLĄSKIE' WHERE WOJEWODZTWO= 'DOLNOŚL„SKIE';
UPDATE sprzedaz_luty2024 SET WOJEWODZTWO='ŁÓDZKIE' WHERE WOJEWODZTWO= '?ÓDZKIE';

3. Analiza spalania
sqlSELECT s.marka, s.spalanie
FROM kategoria k, samochody s
WHERE k.idkategoria=s.idkategoria AND s.spalanie<10;

5. Statystyki agregujące
sqlSELECT k.nazwa kategoria, s.marka, min(spalanie), max(s.spalanie)
FROM kategoria k, samochody s
WHERE k.idkategoria=s.idkategoria
GROUP BY kategoria, s.marka;

Problemy znane
Kodowanie znaków - niektóre nazwy województw zawierały błędne znaki
Struktura danych - konieczność ręcznego dodawania kluczy obcych
Normalizacja - tabele miesięczne mogłyby być zunifikowane

Możliwe rozszerzenia

 Dodanie analizy trendów rocznych
 Implementacja procedur składowanych
 Dodanie widoków dla częstych zapytań
 Integracja z narzędziami BI
 Dodanie danych o cenach samochodów

Autor
Projekt wykonany jako część zaliczenia z przedmiotu baz danych.
Licencja
Dane pochodzą z portalu dane.gov.pl i są udostępnione na licencji otwartej.

Ostatnia aktualizacja: Styczeń 2025
