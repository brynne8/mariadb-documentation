# Server Locale

The [lc_time_names](/kb/en/server-system-variables/#lc_time_names) server system variable sets the language used by the date and time functions [DAYNAME()](/built-in-functions/date-time-functions/dayname/), [MONTHNAME()](/built-in-functions/date-time-functions/monthname/) and [DATE_FORMAT()](/built-in-functions/date-time-functions/date_format/).

The list of the locales supported by the current MariaDB installation can be obtained via the [LOCALES](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/locales-plugin/) plugin, available since [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/).

MariaDB supports the following locale values:

<table><tbody><tr><th>Locale</th><th>Language</th><th>Country</th></tr>
<tr><td>ar_AE</td><td>Arabic</td><td>United Arab Emirates</td></tr>
<tr><td>ar_BH</td><td>Arabic</td><td>Bahrain</td></tr>
<tr><td>ar_DZ</td><td>Arabic</td><td>Algeria</td></tr>
<tr><td>ar_EG</td><td>Arabic</td><td>Egypt</td></tr>
<tr><td>ar_IN</td><td>Arabic</td><td>India</td></tr>
<tr><td>ar_IQ</td><td>Arabic</td><td>Iraq</td></tr>
<tr><td>ar_JO</td><td>Arabic</td><td>Jordan</td></tr>
<tr><td>ar_KW</td><td>Arabic</td><td>Kuwait</td></tr>
<tr><td>ar_LB</td><td>Arabic</td><td>Lebanon</td></tr>
<tr><td>ar_LY</td><td>Arabic</td><td>Libya</td></tr>
<tr><td>ar_MA</td><td>Arabic</td><td>Morocco</td></tr>
<tr><td>ar_OM</td><td>Arabic</td><td>Oman</td></tr>
<tr><td>ar_QA</td><td>Arabic</td><td>Qatar</td></tr>
<tr><td>ar_SA</td><td>Arabic</td><td>Saudi Arabia</td></tr>
<tr><td>ar_SD</td><td>Arabic</td><td>Sudan</td></tr>
<tr><td>ar_SY</td><td>Arabic</td><td>Syria</td></tr>
<tr><td>ar_TN</td><td>Arabic</td><td>Tunisia</td></tr>
<tr><td>ar_YE</td><td>Arabic</td><td>Yemen</td></tr>
<tr><td>be_BY</td><td>Belarusian</td><td>Belarus</td></tr>
<tr><td>bg_BG</td><td>Bulgarian</td><td>Bulgaria</td></tr>
<tr><td>ca_ES</td><td>Catalan</td><td>Spain</td></tr>
<tr><td>cs_CZ</td><td>Czech</td><td>Czech Republic</td></tr>
<tr><td>da_DK</td><td>Danish</td><td>Denmark</td></tr>
<tr><td>de_AT</td><td>German</td><td>Austria</td></tr>
<tr><td>de_BE</td><td>German</td><td>Belgium</td></tr>
<tr><td>de_CH</td><td>German</td><td>Switzerland</td></tr>
<tr><td>de_DE</td><td>German</td><td>Germany</td></tr>
<tr><td>de_LU</td><td>German</td><td>Luxembourg</td></tr>
<tr><td>en_AU</td><td>English</td><td>Australia</td></tr>
<tr><td>en_CA</td><td>English</td><td>Canada</td></tr>
<tr><td>en_GB</td><td>English</td><td>United Kingdom</td></tr>
<tr><td>en_IN</td><td>English</td><td>India</td></tr>
<tr><td>en_NZ</td><td>English</td><td>New Zealand</td></tr>
<tr><td>en_PH</td><td>English</td><td>Philippines</td></tr>
<tr><td>en_US</td><td>English</td><td>United States</td></tr>
<tr><td>en_ZA</td><td>English</td><td>South Africa</td></tr>
<tr><td>en_ZW</td><td>English</td><td>Zimbabwe</td></tr>
<tr><td>es_AR</td><td>Spanish</td><td>Argentina</td></tr>
<tr><td>es_BO</td><td>Spanish</td><td>Bolivia</td></tr>
<tr><td>es_CL</td><td>Spanish</td><td>Chile</td></tr>
<tr><td>es_CO</td><td>Spanish</td><td>Columbia</td></tr>
<tr><td>es_CR</td><td>Spanish</td><td>Costa Rica</td></tr>
<tr><td>es_DO</td><td>Spanish</td><td>Dominican Republic</td></tr>
<tr><td>es_EC</td><td>Spanish</td><td>Ecuador</td></tr>
<tr><td>es_ES</td><td>Spanish</td><td>Spain</td></tr>
<tr><td>es_GT</td><td>Spanish</td><td>Guatemala</td></tr>
<tr><td>es_HN</td><td>Spanish</td><td>Honduras</td></tr>
<tr><td>es_MX</td><td>Spanish</td><td>Mexico</td></tr>
<tr><td>es_NI</td><td>Spanish</td><td>Nicaragua</td></tr>
<tr><td>es_PA</td><td>Spanish</td><td>Panama</td></tr>
<tr><td>es_PE</td><td>Spanish</td><td>Peru</td></tr>
<tr><td>es_PR</td><td>Spanish</td><td>Puerto Rico</td></tr>
<tr><td>es_PY</td><td>Spanish</td><td>Paraguay</td></tr>
<tr><td>es_SV</td><td>Spanish</td><td>El Salvador</td></tr>
<tr><td>es_US</td><td>Spanish</td><td>United States</td></tr>
<tr><td>es_UY</td><td>Spanish</td><td>Uruguay</td></tr>
<tr><td>es_VE</td><td>Spanish</td><td>Venezuela</td></tr>
<tr><td>et_EE</td><td>Estonian</td><td>Estonia</td></tr>
<tr><td>eu_ES</td><td>Basque</td><td>Basque</td></tr>
<tr><td>fi_FI</td><td>Finnish</td><td>Finland</td></tr>
<tr><td>fo_FO</td><td>Faroese</td><td>Faroe Islands</td></tr>
<tr><td>fr_BE</td><td>French</td><td>Belgium</td></tr>
<tr><td>fr_CA</td><td>French</td><td>Canada</td></tr>
<tr><td>fr_CH</td><td>French</td><td>Switzerland</td></tr>
<tr><td>fr_FR</td><td>French</td><td>France</td></tr>
<tr><td>fr_LU</td><td>French</td><td>Luxembourg</td></tr>
<tr><td>gl_ES</td><td>Galician</td><td>Spain</td></tr>
<tr><td>gu_IN</td><td>Gujarati</td><td>India</td></tr>
<tr><td>he_IL</td><td>Hebrew</td><td>Israel</td></tr>
<tr><td>hi_IN</td><td>Hindi</td><td>India</td></tr>
<tr><td>hr_HR</td><td>Croatian</td><td>Croatia</td></tr>
<tr><td>hu_HU</td><td>Hungarian</td><td>Hungary</td></tr>
<tr><td>id_ID</td><td>Indonesian</td><td>Indonesia</td></tr>
<tr><td>is_IS</td><td>Icelandic</td><td>Iceland</td></tr>
<tr><td>it_CH</td><td>Italian</td><td>Switzerland</td></tr>
<tr><td>it_IT</td><td>Italian</td><td>Italy</td></tr>
<tr><td>ja_JP</td><td>Japanese</td><td>Japan</td></tr>
<tr><td>ko_KR</td><td>Korean</td><td>Republic of Korea</td></tr>
<tr><td>lt_LT</td><td>Lithuanian</td><td>Lithuania</td></tr>
<tr><td>lv_LV</td><td>Latvian</td><td>Latvia</td></tr>
<tr><td>mk_MK</td><td>Macedonian</td><td>FYROM</td></tr>
<tr><td>mn_MN</td><td>Mongolia</td><td>Mongolian</td></tr>
<tr><td>ms_MY</td><td>Malay</td><td>Malaysia</td></tr>
<tr><td>nb_NO</td><td>Norwegian(Bokmål)</td><td>Norway</td></tr>
<tr><td>nl_BE</td><td>Dutch</td><td>Belgium</td></tr>
<tr><td>nl_NL</td><td>Dutch</td><td>The Netherlands</td></tr>
<tr><td>no_NO</td><td>Norwegian</td><td>Norway</td></tr>
<tr><td>pl_PL</td><td>Polish</td><td>Poland</td></tr>
<tr><td>pt_BR</td><td>Portugese</td><td>Brazil</td></tr>
<tr><td>pt_PT</td><td>Portugese</td><td>Portugal</td></tr>
<tr><td>rm_CH</td><td>Romansh</td><td>Switzerland</td></tr>
<tr><td>ro_RO</td><td>Romanian</td><td>Romania</td></tr>
<tr><td>ru_RU</td><td>Russian</td><td>Russia</td></tr>
<tr><td>ru_UA</td><td>Russian</td><td>Ukraine</td></tr>
<tr><td>sk_SK</td><td>Slovak</td><td>Slovakia</td></tr>
<tr><td>sl_SI</td><td>Slovenian</td><td>Slovenia</td></tr>
<tr><td>sq_AL</td><td>Albanian</td><td>Albania</td></tr>
<tr><td>sr_YU</td><td>Serbian</td><td>Yugoslavia</td></tr>
<tr><td>sv_FI</td><td>Swedish</td><td>Finland</td></tr>
<tr><td>sv_SE</td><td>Swedish</td><td>Sweden</td></tr>
<tr><td>ta_IN</td><td>Tamil</td><td>India</td></tr>
<tr><td>te_IN</td><td>Telugu</td><td>India</td></tr>
<tr><td>th_TH</td><td>Thai</td><td>Thailand</td></tr>
<tr><td>tr_TR</td><td>Turkish</td><td>Turkey</td></tr>
<tr><td>uk_UA</td><td>Ukrainian</td><td>Ukraine</td></tr>
<tr><td>ur_PK</td><td>Urdu</td><td>Pakistan</td></tr>
<tr><td>vi_VN</td><td>Vietnamese</td><td>Viet Nam</td></tr>
<tr><td>zh_CN</td><td>Chinese</td><td>China</td></tr>
<tr><td>zh_HK</td><td>Chinese</td><td>Hong Kong</td></tr>
<tr><td>zh_TW</td><td>Chinese</td><td>Taiwan Province of China</td></tr>
</tbody></table>

## Examples

Setting the [lc_time_names](/kb/en/server-system-variables/#lc_time_names) and [lc_messages](/kb/en/server-system-variables/#lc_messages) variables to localize the units of date and time, and the server error messages.

```sql
SELECT DAYNAME('2013-04-01'), MONTHNAME('2013-04-01');
+-----------------------+-------------------------+
| DAYNAME('2013-04-01') | MONTHNAME('2013-04-01') |
+-----------------------+-------------------------+
| Monday                | April                   |
+-----------------------+-------------------------+

SET lc_time_names = 'fr_CA';

SELECT DAYNAME('2013-04-01'), MONTHNAME('2013-04-01');
+-----------------------+-------------------------+
| DAYNAME('2013-04-01') | MONTHNAME('2013-04-01') |
+-----------------------+-------------------------+
| lundi                 | avril                   |
+-----------------------+-------------------------+

SELECT blah;
ERROR 1054 (42S22): Unknown column 'blah' in 'field' list'

SET lc_messages = 'nl_NL';

SELECT blah;
ERROR 1054 (42S22): Onbekende kolom 'blah' in field list
```