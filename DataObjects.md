---
title: SeLite Data Objects
layout: default
---

~~~
    SeLiteData.Storage
      ^
    SeLiteData.Db
      ^
    SeLiteData.Table
      ^
/-> SeLiteData.RecordSetFormula
|
|   RecordSetHolder <-----------------------<-------------------------------------------------------------------------<-\
\--<- .formula                               \                                                                          |
      .recordSet   -> SeLiteData.RecordSet   |                                                                          |
                        .recordSetHolder ->--/                                                                          |
                        [ record[formula.indexBy] ][ record[formula.subIndexBy] - optional ] -> SeLiteData.Record ->-\  |
                                                                                                                     |  |
      .holders[primaryKeyValue] -> RecordHolder <-------------------------------------------------<-\                |  |
                                                                                                    |                |  |
                                     .record   ->  SeLiteData.Record                                |     <--------<-/  |
                                                     [ SeLiteData.Record.RECORD_TO_HOLDER_FIELD ]->-/                   |
                                                     [ string fieldName ] -> fieldValue                                 |
                                     .recordSetHolder ->----------------------------------------------------------------/
                                     .original ->  anonymous object, partial copy of .record  <--\
                                                                                                 |
      .originals[primaryKeyValue] -> .holders[primaryKeyValue].original ->-----------------------/
~~~