<!-- <style>
 p,li {
    font-size: 12pt;
  }
</style>  -->

<!-- <style>
 pre {
    font-size: 8pt;
  }
</style>  -->


---


**Temat:**
System do obsługi stacji benzynowej

**Autorzy:** 

Rafał Dubiel, 
Daniel Antolak, 
Filip Adamus, 
Filip Przepiórka.

--- 

# 1.  Zakres i krótki opis systemu

Projekt ma na celu stworzenie systemu do obsługi procesu tankowania na stacji benzynowej. W projektowanej bazie danych będą rejestrowane wszelkie informacje, wymagane do poprawnego funkcjonowania stacji. 
Wszystkie rekordy będą odpowiednio zapisywane w utworzonych tabelach pod unikalnymi wierszami. Dodatko w systemie zostaną zaimplementowanych funkcje, widoki, procedury,
mające na celu ułatwnienie czynności związanych z obsługą stacji oraz prowadzeniem analizy danych.

Klient przyjeżdża na stacje paliw, wybiera odpowiedni, czynny dystrybutor i rodzaj paliwa, które chce zatankować. Wybranie nieczynnego dystrybutora, uniemożliwi kontynuowanie procesu.
Podnosi pistolet i zaczyna proces tankowania. Po zakończeniu czynności, musi odłożyć pistolet na swoje miejsce. Bez zakończenia procesu, klient nie może zapłacić za zatankowane paliwo.
Odłożenie pistoletu jest wyznacznikiem zakończenia tankowania. Po odłożeniu, klient może przystąpić do płacenia.
Po opłaceniu rachunku przez klienta, zapisana zostanie kwota transakcji, przyznana zniżka , którą klient może posiadać, data, numer dystrybutora z którego pobrano paliwo, 
pracownika który obsłużył zamówienie oraz numer samochodu.
Klient ma możliwość zakupu paliwa na paragon lub fakturę. Do wzięcia faktury system wymaga wprowadzenia numeru NIP. 
Klient może podać numery rejestracyjne pojazdu, nie jest to jednak wymogiem.

System będzie pozwalał na manipulacje cenami paliwa przez pracowników, otrzymanych od
członka zarządu stacji.
Zmiany cen paliwa są rejestrowane w bazie wraz z datami, w których te zmiany zostały wprowadzone.

System przechowywać będzie informacje na temat
dostawców paliwa, dystrybutorów, klientów, transakcji oraz pracowników stacji. Będzie możliwość
generowania raportów oraz zarządzania transakcjami. System będzie
dostępny dla pracowników stacji paliw oraz właścicieli.




# 2.	Wymagania i funkcje systemu

System powinien rejestrować wszystkie informacje dotyczące sprzedaży paliwa, klientów, pracowników, dostaw, rozliczeń. 

System powinien zawierać odpowiednie procedury, widoki i funkcje służące organizacji, analizie i zarządzaniu bazą danych. 

System powinien umożliwiać zapisanie danych firmy do faktury poprzez unikalny numer NIP, aby przyspieszyć proces przy następnej sprzedaży.

Jako klient, chcę mieć możliwość otrzymania faktury lub paragonu za kupione paliwo.

Jako klient, chcę mieć możliwość wyboru co do podawania numeru rejestracyjnego do faktury.

Jako sprzedawca chcę mieć możliwość uzyskania raportu sprzedaży z konkretnego dnia.

System powinien rejestrować dostawy paliwa wraz z konkretną datą, ceną, ilością i typem paliwa oraz dostawcą.

System powinien rejestrować jaki typ paliwa jest tankowany.

System powinien dać możliwość wygenerowania wszystkich informacji z konkretnych sprzedaży.

System powinien umożliwić monitorowanie zapasów paliwa.

System powinien rejestrować zmiany cen paliwa.

Jako pracownik, chcę wiedzieć jaka cena paliwa była w określonym dniu.

# 3.	Projekt bazy danych

## Diagram przedstawiający schemat bazy danych

![image](https://github.com/fprzepio/Petrol-station-Database/assets/132128402/070664b4-d67d-4bc1-a389-04d8136abe9a)

## Opis poszczególnych tabel

<br>

Nazwa tabeli: petrol_history
- Opis: Tabela przechowująca historię cen paliwa.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| petrol_history_id | int | PK, Numer ID historii paliwa |
| petrol_id | int |FK do tabeli petrol, Numer ID paliwa |
| price | money | Historyczna cena paliwa |
| date | datetime | Data |

<br>

Nazwa tabeli: petrol
- Opis: Tabela zawierająca informacje o paliwach.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| petrol_id | int | PK, Numer ID paliwa |
| supplier_id | int | FK do tabeli supplier, Numer ID dostawcy paliwa |
| name | varchar(50) | Nazwa paliwa |
| in_stock | float | Aktualny stan paliwa |
| price | money | Aktualna cena paliwa |

<br>

Nazwa tabeli: supplier
- Opis: Tabela zawierająca dokładne dane dostawców paliwa.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| supplier_id | int | PK, Numer ID dostawcy
| company_name | varchar(50) | Nazwa dostawcy |
| postal_code | varchar(10) | Kod pocztowy |
| address | varchar(50) | Adres siedziby firmy |
| city | varchar(30) | Miasto dostawcy |
| country | varchar(30) | Państwo dostawcy |
| phone | varchar(20) | Numer telefonu dostawcy |

<br>

Nazwa tabeli: pump
- Opis: Tabela reprezentująca konkretne pistolety paliwa oraz ich dystrybutory.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| pump_id | int | PK, Numer ID pistoletu |
| petrol_id | int | FK do tabeli petrol, Numer ID paliwa |
| distributor_no | int | Numer dystrybutora |
| status | varchar(50) | FK do tabeli status, Status pompy |

<br>

Nazwa tabeli: transaction
- Opis:  Tabela zawierające informacje o transakcjach kupna.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| transaction_id | int | PK, Numer ID transakcji |
| pump_id | int | FK do tabeli pump, Numer ID pistoletu |
| amount | float | Ilość zakupionego paliwa |
| employee_id | int | FK do tabeli employee, Numer ID pracownika |
| date | datetime | Data transakcji |
| discount_id | int | Numer ID rabatu |

<br>

Nazwa tabeli: employee
- Opis: Tabela zawierająca informacje o pracownikach.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| employee_id | int | PK, Numer ID pracownika |
| firstname | varchar(30) | Imię pracownika |
| lastname | varchar(30) | Nazwisko pracownika |
| title | varchar(20) | Stanowisko pracownika |
| address | varchar(20) | Adres pracownika |
| city | varchar(20) | Miasto pracownika |
| postal_code | varchar(10) | Kod pocztowy |
| country | varchar(20) | Państwo pracownika |
| phone | varchar(20) | Numer telefonu |

<br>

Nazwa tabeli: invoice
- Opis: Tabela zawierająca dane faktur.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| invoice_id | int | PK, Numer ID faktury |
| transaction_id | int | FK do tabeli transaction, Numer ID transakcji |
| NIP | varchar(10) | Numer NIP firmy |
| plate | varchar(20) | Numer rejestracyjny (zezwala na wartość NULL) |

<br>

Nazwa tabeli: discounts
- Opis: Tabela zawierająca specyfikację rabatów.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| discount_id | int | PK, Numer ID rabatu |
| discount_name | varchar(50) | Nazwa rabatu |
| value | float | Wartość rabatu |

<br>

Nazwa tabeli: discount_history
- Opis: Tabela zawierająca historię rabatów w czasie.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| discount_history_id | int | PK, Numer ID historii rabatu |
| discount_id | int | FK do tabeli discounts, Numer ID rabatu |
| value | float | Wartość rabatu |
| start_date | datetime | Data rozpoczęcia rabatu |
| end_date | datetime | Data zakończenia rabatu |

<br>

Nazwa tabeli: distributor
- Opis: Tabela zawierająca informację o dystrybutorach.

| Nazwa atrybutu | Typ  | Opis/Uwagi |
|----------------|------|------------|
| distributor_no | int | PK, Numer dystrybutora |
| status | varchar(50) | FK do tabeli status, status |


<br>



# 4.	Implementacja

## Kod poleceń DDL

(dla każdej tabeli należy wkleić kod DDL polecenia tworzącego tabelę)

<br>
Tabela: discount_history
<br>

```sql
CREATE TABLE dbo.discount_history (
	discount_history_id INT NOT NULL,
	discount_id INT NOT NULL,
	value FLOAT NOT NULL,
	start_date DATETIME NOT NULL,
	end_date DATETIME NOT NULL,
	CONSTRAINT PK_discount_history PRIMARY KEY CLUSTERED (discount_history_id ASC)
) ON [PRIMARY];

ALTER TABLE dbo.discount_history WITH CHECK ADD CONSTRAINT FK_discount_history_discounts1 FOREIGN KEY(discount_id)
REFERENCES dbo.discounts (discount_id);

ALTER TABLE dbo.discount_history CHECK CONSTRAINT FK_discount_history_discounts1;

```

<br>
Tabela: discounts
<br>

```sql
CREATE TABLE dbo.discounts (
	discount_id INT NOT NULL,
	discount_name VARCHAR(50) NOT NULL,
	value FLOAT NOT NULL,
	CONSTRAINT PK_discounts PRIMARY KEY CLUSTERED (discount_id ASC)
) ON [PRIMARY];

```
<br>
Tabela: distributor
<br>

```sql
CREATE TABLE dbo.distributor (
	distributor_no INT NOT NULL,
	status VARCHAR(50) NOT NULL,
	CONSTRAINT PK_distributor PRIMARY KEY CLUSTERED (distributor_no ASC)
) ON [PRIMARY];

```
<br>
Tabela: employee
<br>

```sql
CREATE TABLE dbo.employee (
	employee_id INT NOT NULL,
	firstname VARCHAR(30) NOT NULL,
	lastname VARCHAR(30) NOT NULL,
	title VARCHAR(20) NOT NULL,
	address VARCHAR(20) NOT NULL,
	city VARCHAR(20) NOT NULL,
	postal_code VARCHAR(10) NOT NULL,
	country VARCHAR(20) NOT NULL,
	phone VARCHAR(20) NOT NULL,
	CONSTRAINT PK_employee PRIMARY KEY CLUSTERED (employee_id ASC)
) ON [PRIMARY];

```
<br>
Tabela: invoice
<br>

```sql
CREATE TABLE dbo.invoice (
	invoice_id INT NOT NULL,
	transaction_id INT NOT NULL,
	NIP VARCHAR(10) NOT NULL,
	plate VARCHAR(20) NULL,
	CONSTRAINT PK_invoice PRIMARY KEY CLUSTERED (invoice_id ASC)
) ON [PRIMARY];

ALTER TABLE dbo.invoice WITH CHECK ADD CONSTRAINT FK_invoice_transaction
FOREIGN KEY (transaction_id) REFERENCES dbo.transaction (transaction_id);

ALTER TABLE dbo.invoice CHECK CONSTRAINT FK_invoice_transaction;


```
<br>
Tabela: petrol
<br>

```sql
CREATE TABLE dbo.petrol (
	petrol_id INT NOT NULL,
	supplier_id INT NOT NULL,
	name VARCHAR(50) NOT NULL,
	in_stock FLOAT NOT NULL,
	price MONEY NOT NULL,
	CONSTRAINT PK_petrol PRIMARY KEY CLUSTERED (petrol_id ASC)
) ON [PRIMARY];


```
<br>
Tabela: petrol_history
<br>

```sql
CREATE TABLE dbo.petrol_history (
	petrol_history_id INT NOT NULL,
	petrol_id INT NOT NULL,
	price MONEY NOT NULL,
	date DATETIME NOT NULL,
 CONSTRAINT PK_petrol_history PRIMARY KEY CLUSTERED (petrol_history_id ASC)
) ON [PRIMARY];

ALTER TABLE dbo.petrol_history  WITH CHECK ADD CONSTRAINT FK_petrol_history_petrol FOREIGN KEY(petrol_id)
REFERENCES dbo.petrol (petrol_id)

ALTER TABLE dbo.petrol_history CHECK CONSTRAINT FK_petrol_history_petrol

```
<br>
Tabela: pump
<br>

```sql
CREATE TABLE dbo.pump (
	pump_id INT NOT NULL,
	petrol_id INT NOT NULL,
	distributor_no INT NOT NULL,
	status VARCHAR(50) NOT NULL,
	CONSTRAINT PK_pump PRIMARY KEY CLUSTERED (pump_id ASC)
) ON [PRIMARY];

ALTER TABLE dbo.pump WITH CHECK ADD CONSTRAINT FK_pump_distributor FOREIGN KEY(distributor_no)
REFERENCES dbo.distributor (distributor_no);

ALTER TABLE dbo.pump CHECK CONSTRAINT FK_pump_distributor;

ALTER TABLE dbo.pump WITH CHECK ADD CONSTRAINT FK_pump_petrol FOREIGN KEY(petrol_id)
REFERENCES dbo.petrol (petrol_id);

ALTER TABLE dbo.pump CHECK CONSTRAINT FK_pump_petrol;

```

<br>
Tabela: supplier
<br>

```sql
CREATE TABLE dbo.supplier (
	supplier_id INT NOT NULL,
	company_name VARCHAR(50) NOT NULL,
	postal_code VARCHAR(10) NOT NULL,
	address VARCHAR(50) NOT NULL,
	city VARCHAR(30) NOT NULL,
	country VARCHAR(30) NOT NULL,
	phone VARCHAR(20) NOT NULL,
	CONSTRAINT PK_supplier PRIMARY KEY CLUSTERED (supplier_id ASC)
) ON [PRIMARY];
```

<br>
Tabela: supply
<br>

```sql
CREATE TABLE dbo.supply (
	supply_id INT NOT NULL,
	supplier_id INT NOT NULL,
	amount FLOAT NOT NULL,
	date DATETIME NOT NULL,
	petrol_id INT NOT NULL,
	price MONEY NOT NULL,
	CONSTRAINT PK_supply PRIMARY KEY CLUSTERED (supply_id ASC)
) ON [PRIMARY];

ALTER TABLE dbo.supply WITH CHECK ADD CONSTRAINT FK_supply_petrol FOREIGN KEY(petrol_id)
REFERENCES dbo.petrol (petrol_id);

ALTER TABLE dbo.supply CHECK CONSTRAINT FK_supply_petrol;

ALTER TABLE dbo.supply WITH CHECK ADD CONSTRAINT FK_supply_supplier FOREIGN KEY(supplier_id)
REFERENCES dbo.supplier (supplier_id);

ALTER TABLE dbo.supply CHECK CONSTRAINT FK_supply_supplier;

```

<br>
Tabela: transaction
<br>

```sql
CREATE TABLE dbo.transaction (
	transaction_id INT NOT NULL,
	pump_id INT NOT NULL,
	amount FLOAT NOT NULL,
	employee_id INT NOT NULL,
	[date] DATETIME NOT NULL,
	discount_id INT NULL,
	CONSTRAINT PK_transaction PRIMARY KEY CLUSTERED (transaction_id ASC)
) ON [PRIMARY];

ALTER TABLE dbo.transaction WITH CHECK ADD CONSTRAINT FK_transaction_discounts FOREIGN KEY(discount_id)
REFERENCES dbo.discounts (discount_id);

ALTER TABLE dbo.transaction CHECK CONSTRAINT FK_transaction_discounts;

ALTER TABLE dbo.transaction WITH CHECK ADD CONSTRAINT FK_transaction_Employee FOREIGN KEY(employee_id)
REFERENCES dbo.employee (employee_id);

ALTER TABLE dbo.transaction CHECK CONSTRAINT FK_transaction_Employee;

ALTER TABLE dbo.transaction WITH CHECK ADD CONSTRAINT FK_transaction_pump1 FOREIGN KEY(pump_id)
REFERENCES dbo.pump (pump_id);

ALTER TABLE dbo.transaction CHECK CONSTRAINT FK_transaction_pump1;

```

## Widoki

(dla każdego widoku należy wkleić kod polecenia definiującego widok wraz z komentarzem)

## Procedury/funkcje

(dla każdej procedury/funkcji należy wkleić kod polecenia definiującego procedurę wraz z komentarzem)

## Triggery

(dla każdego triggera należy wkleić kod polecenia definiującego trigger wraz z komentarzem)



[wzor_dok.md](https://github.com/FilipAdamus03/Petrol-Station-Database/files/14943695/wzor_dok.md)
