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

Tabela samochody powiązana z kategoria (klucz obcy), tabele sprzedaży powiązane z województwa (klucz obcy)

Funkcjonalności
1. Zarządzanie danymi podstawowymi: tworzenie i zarządzanie kategoriami pojazdów, dodawanie informacji o samochodach (marka, model, spalanie), zarządzanie słownikami województw i miesięcy

2. Analiza sprzedaży

Analiza sprzedaży według województw, porównanie sprzedaży między miesiącami, analiza konkretnych marek (szczególnie Mazda),statystyki min/max sprzedaży

3. Zaawansowane zapytania

Łączenie danych z wielu miesięcy, grupowanie i agregacje, sortowanie wyników, warunki logiczne z HAVING

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


Zaimportuj plik SQL do MySQL, 
uruchom skrypt w MySQL Workbench lub przez wiersz poleceń


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
Kodowanie znaków - niektóre nazwy województw zawierały błędne znaki, struktura danych - konieczność ręcznego dodawania kluczy obcych, normalizacja - tabele miesięczne mogłyby być zunifikowane


Autor
Projekt wykonany jako część zaliczenia z przedmiotu baz danych.
Licencja: 
dane pochodzą z portalu dane.gov.pl i są udostępnione na licencji otwartej.

Ostatnia aktualizacja: Styczeń 2025
