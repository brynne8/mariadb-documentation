# Can't create a table starting with a capital letter. All tables are lower case-

Hi,

I was playing around with my MariaDB ColumnStore and I noticed the I am not able to create tables/databases with capital letters, it seems that only lowercase can be used:

CREATE TABLE TEST
 ( Codice_Regione INT (11),
Codice_Citta_Metropolitana VARCHAR (50),
Codice_Provincia INT (11),
Progressivo_del_Comune INT (11),
Codice_Comune_formato_alfanumerico VARCHAR (25),
Denominazione_corrente VARCHAR (100),
Denominazione_altra_lingua VARCHAR (300),
Codice_Ripartizione_Geografica INT (11),
Ripartizione_geografica VARCHAR (25),
Denominazione_regione VARCHAR (125),
Denominazione_Citta_metropolitana VARCHAR (25),
Denominazione_provincia VARCHAR (125),
Flag_Comune_capoluogo_di_provincia VARCHAR (25),
Sigla_automobilistica VARCHAR (2),
Codice_Comune_formato_numerico INT (11),
Codice_Comune_numerico_con_110_province_dal_2010al_2016 INT (11),
Codice_Comune_numerico_con_107_province_dal_2006al_2009 INT (11),
Codice_Comune_numerico_con_103_province_dal_1995al_2005 INT (11),
Codice_Catastale_del_comune VARCHAR (25),
Popolazione_legale_2011_09102011 BIGINT,
Codice_NUTS12010 VARCHAR (25),
Codice_NUTS22010 VARCHAR (25),
Codice_NUTS32010 VARCHAR (25),
Codice_NUTS12006 VARCHAR (25),
Codice_NUTS22006 VARCHAR (25),
Codice_NUTS32006 VARCHAR (25)
)

My CS Version is 1.1.2 install/10.2.10-MariaDB-log and my standard mariadb is 10.2.12-MariaDB

Is this by design?

Thanks for the info.

Luca