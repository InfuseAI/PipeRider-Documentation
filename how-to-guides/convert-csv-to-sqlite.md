---
description: How to convert a csv file to a SQLite database.
---

# Convert CSV to SQLite

### Prepare CSV file

You can use your own csv file or use the example data below.

Copy the data and save it as a `vgsales.csv` file.

```csv
Rank,Name,Platform,Year,Genre,Publisher,NA_Sales,EU_Sales,JP_Sales,Other_Sales,Global_Sales
1,Wii Sports,Wii,2006,Sports,Nintendo,41.49,29.02,3.77,8.46,82.74
2,Super Mario Bros.,NES,1985,Platform,Nintendo,29.08,3.58,6.81,0.77,40.24
3,Mario Kart Wii,Wii,2008,Racing,Nintendo,15.85,12.88,3.79,3.31,35.82
4,Wii Sports Resort,Wii,2009,Sports,Nintendo,15.75,11.01,3.28,2.96,33
5,Pokemon Red/Pokemon Blue,GB,1996,Role-Playing,Nintendo,11.27,8.89,10.22,1,31.37
6,Tetris,GB,1989,Puzzle,Nintendo,23.2,2.26,4.22,0.58,30.26
7,New Super Mario Bros.,DS,2006,Platform,Nintendo,11.38,9.23,6.5,2.9,30.01
8,Wii Play,Wii,2006,Misc,Nintendo,14.03,9.2,2.93,2.85,29.02
9,New Super Mario Bros. Wii,Wii,2009,Platform,Nintendo,14.59,7.06,4.7,2.26,28.62
```

### Prepare Database and Table

In the terminal, run the command to create a new database, `vgsales.db`:

```shell
sqlite3 vgsales.db
```

then you will be in the sqlite command mode.

```
sqlite>
```

In the command mode, run the query to create a table `sales` with the defined schema.

{% hint style="info" %}
Define the appropriate schema to your table. The schema will affect what types of assertions you could use.
{% endhint %}

```sql
sqlite> create table sales (
	Rank INTEGER PRIMARY KEY,
	Name TEXT,
	Platform TEXT,
	Year TEXT,
	Genre TEXT,
	Publisher TEXT,
	NA_Sales REAL,
	EU_Sales REAL,
	JP_Sales REAL,
	Other_Sales REAL,
	Global_Sales REAL
);
```

Verify the schema

```
sqlite> .schema
```

```
sqlite> .schema
CREATE TABLE sales (
Rank INTEGER PRIMARY KEY,
Name TEXT,
Platform TEXT,
Year TEXT,
Genre TEXT,
Publisher TEXT,
NA_Sales REAL,
EU_Sales REAL,
JP_Sales REAL,
Other_Sales REAL,
Global_Sales REAL
);
```

### Import CSV

In the sqlite command mode, to import the CSV file without loading the first row which is the header.

```
sqlite>.import --csv --skip 1 vgsales.csv sales
```

Verify the data

```sql
sqlite>select * from sales;
```

```sql
sqlite> select * from sales;
1|Wii Sports|Wii|2006|Sports|Nintendo|41.49|29.02|3.77|8.46|82.74
2|Super Mario Bros.|NES|1985|Platform|Nintendo|29.08|3.58|6.81|0.77|40.24
3|Mario Kart Wii|Wii|2008|Racing|Nintendo|15.85|12.88|3.79|3.31|35.82
4|Wii Sports Resort|Wii|2009|Sports|Nintendo|15.75|11.01|3.28|2.96|33
5|Pokemon Red/Pokemon Blue|GB|1996|Role-Playing|Nintendo|11.27|8.89|10.22|1|31.37
6|Tetris|GB|1989|Puzzle|Nintendo|23.2|2.26|4.22|0.58|30.26
7|New Super Mario Bros.|DS|2006|Platform|Nintendo|11.38|9.23|6.5|2.9|30.01
8|Wii Play|Wii|2006|Misc|Nintendo|14.03|9.2|2.93|2.85|29.02
9|New Super Mario Bros. Wii|Wii|2009|Platform|Nintendo|14.59|7.06|4.7|2.26|28.62q
```
