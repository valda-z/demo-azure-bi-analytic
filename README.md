# Hlasovací portál s online vzhodnocováním dat

??? Nějaký popis ???

Genereate QR codes for functions URL in http://goqr.me 

### Kód funkce pro hlasování "YES"

```javascript
module.exports = function (context, req) {
    var timeStamp = new Date().toISOString();
    context.bindings.outputEventHubMessage = {
        created: new Date(),
        vote: 'YES'
    };
    context.done();
};
```

### Kód funkce pro hlasování "NO"

```javascript
module.exports = function (context, req) {
    var timeStamp = new Date().toISOString();
    context.bindings.outputEventHubMessage = {
        created: new Date(),
        vote: 'NO'
    };
    context.done();
};
```

### Kód SQL dotazu pro Stream Analytics

```sql
SELECT
    1 as person,
    CAST(DATEADD(hour,1,created) as datetime) as Time,
    DATENAME (year, DATEADD(hour,1,created)) as Year,
    DATENAME (month, DATEADD(hour,1,created)) as Month,
    DATENAME (day, DATEADD(hour,1,created)) as Day,
    DATENAME (weekday, DATEADD(hour,1,created)) as WeekDay,
    DATENAME (hour, DATEADD(hour,1,created)) as Hour,
    DATENAME (minute, DATEADD(hour,1,created)) as Minute,
    UPPER(vote) as vote,
    CASE WHEN UPPER(vote)='YES' THEN 1 ELSE 0 END as voteyes,
    CASE WHEN UPPER(vote)='NO' THEN 1 ELSE 0 END as voteno
INTO
    [topowerbi]
FROM
    [fromeventhub]
```
