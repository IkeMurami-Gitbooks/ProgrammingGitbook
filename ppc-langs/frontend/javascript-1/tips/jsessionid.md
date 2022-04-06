# JSessionID

```
- JSessionID: JSESSIONID=SESSION_ID!PRIMARY_JVMID_HASH!SECONDARY_JVM_HASH!CREATION_TIME <- WebLogic format??
    Но вообще, это, походу зависит от реализации конкретной
    WebSphere: <your_session>:<websphere_clone_id>, (JSESSIONID=000024N1ZDMZZH022EANUW2ZO5I:u7078j8m)
               "JSESSIONID=" + request.getSession().getId()

Про PaymentService: https://stackoverflow.com/questions/27039197/test-a-hostapduservice-with-robolectric
```
