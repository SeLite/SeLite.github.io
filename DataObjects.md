---
title: SeLite Data Objects
layout: default
---

<pre>
SeLiteData.Storage<br>
^<br>
SeLiteData.Db<br>
^<br>
SeLiteData.Table<br>
^<br>
/-> SeLiteData.RecordSetFormula<br>
|<br>
|   RecordSetHolder <-----------------------<-------------------------------------------------------------------------<-\<br>
\--<- .formula                               \                                                                          |<br>
.recordSet   -> SeLiteData.RecordSet   |                                                                          |<br>
.recordSetHolder ->--/                                                                          |<br>
[ record[formula.indexBy] ][ record[formula.subIndexBy] - optional ] -> SeLiteData.Record ->-\  |<br>
|  |<br>
.holders[primaryKeyValue] -> RecordHolder <-------------------------------------------------<-\                |  |<br>
|                |  |<br>
.record   ->  SeLiteData.Record                                |     <--------<-/  |<br>
[ SeLiteData.Record.RECORD_TO_HOLDER_FIELD ]->-/                   |<br>
[ string fieldName ] -> fieldValue                                 |<br>
.recordSetHolder ->----------------------------------------------------------------/<br>
.original ->  anonymous object, partial copy of .record  <--\<br>
|<br>
.originals[primaryKeyValue] -> .holders[primaryKeyValue].original ->-----------------------/<br>
<br>
</pre>