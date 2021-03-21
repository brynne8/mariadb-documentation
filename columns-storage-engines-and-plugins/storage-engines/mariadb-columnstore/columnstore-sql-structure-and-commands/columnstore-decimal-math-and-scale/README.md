# ColumnStore Decimal Math and Scale

MariaDB ColumnStore has the ability to change intermediate decimal mathematical results from decimal type to double. The decimal type has approximately 17-18 digits of precision, but a smaller maximum range.  Whereas the double type has approximately 15-16 digits of precision, but a much larger maximum range. 
In typical mathematical and scientific applications, the ability to avoid overflow in intermediate results with double math is likely more beneficial than the additional two digits of precisions. In banking applications, however, it may be more appropriate to leave in the default decimal setting to ensure accuracy to the least significant digit.

### Enable/Disable decimal to double math

The infinidb_double_for_decimal_math variable is used to control the data type for intermediate decimal results. This decimal for double math may be set as a default for the instance, set at the session level, or at the statement level by toggling this variable on and off.

To enable/disable the use of the decimal to double math at the session level, the following command is used. Once the session has ended, any subsequent session will return to the default for the instance.

```sql
set infinidb_double_for_decimal_math = on
```

where n is:

- off (disabled, default)
- on (enabled)

### ColumnStore decimal scale

ColumnStore has the ability to support varied internal precision on decimal calculations. <em>infinidb_decimal_scale</em> is used internally by the ColumnStore engine to control how many significant digits to the right of the decimal point are carried through in suboperations on calculated columns. If, while running a query, you receive the message ‘aggregate overflow’, try reducing <em>infinidb_decimal_scale</em> and running the query again. Note that,as you decrease <em>infinidb_decimal_scale</em>, you may see reduced accuracy in the least significant digit(s) of a returned calculated column. <em>infinidb_use_decimal_scale</em> is used internally by the ColumnStore engine to turn the use of this internal precision on and off. These two system variables may be set as a default for the instance or set at the session level.

#### Enable/disable decimal scale

To enable/disable the use of the decimal scale at the session level, the following command is used. Once the session has ended, any subsequent session will return to the default for the instance.

```sql
set infinidb_use_decimal_scale = on
```

where <em>n</em> is off (disabled) or on (enabled).

#### Set decimal scale level

To set the decimal scale at the session level, the following command is used. Once the session has ended, any subsequent session will return to the default for the instance.

```sql
set infinidb_decimal_scale = n
```

where <em>n</em> is the amount of precision desired for calculations.