# L10.3 - recoverability
serializability helps to ensure isolation and consistency of a schedule
![[Pasted image 20250413025359.png]]

Casecadeless Schedules
* For each pair of transactions Ti and Tj such that Tj reads a data item previously written by Ti, the commit operation of Ti appears before the read operation of Tj
* every cascadeless schedule is also recoverable
* it is desirable to restrict the schedule to those that are cascadeless
![[Pasted image 20250413025622.png]]

## Release save ponit
if reseale is used, cannot rollback behind that

