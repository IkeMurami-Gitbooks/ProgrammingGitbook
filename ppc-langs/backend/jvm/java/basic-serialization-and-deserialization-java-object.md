# Basic Serialization and Deserialization Java Object

Serializable Java Object:

```java
package lab.actions.common.serializable;

import java.io.Serializable;

// https://0af400fd04dde586c05961210095004e.web-security-academy.net/backup/AccessTokenUser.java
public class AccessTokenUser implements Serializable
{
    private final String username;
    private final String accessToken;

    public AccessTokenUser(String username, String accessToken)
    {
        this.username = username;
        this.accessToken = accessToken;
    }

    public String getUsername()
    {
        return username;
    }

    public String getAccessToken()
    {
        return accessToken;
    }
}

```

Main Class:

```java
import lab.actions.common.serializable.AccessTokenUser;

import java.io.*;
import java.util.Base64;

public class Main {
    public static void main(String[] args) throws IOException,
            ClassNotFoundException {
        AccessTokenUser token = new AccessTokenUser("wiener", "111token111");
        String token_ser_b64 = toString( token );
        System.out.println("Serialized token!: " + token_ser_b64);

        token_ser_b64 = "rO0ABXNyAC9sYWIuYWN0aW9ucy5jb21tb24uc2VyaWFsaXphYmxlLkFjY2Vzc1Rva2VuVXNlchlR/OUSJ6mBAgACTAALYWNjZXNzVG9rZW50ABJMamF2YS9sYW5nL1N0cmluZztMAAh1c2VybmFtZXEAfgABeHB0ACBwdzVvdWJocjFsODVteHZjaHhtZ3BqNDV1dHl5b2l1M3QABndpZW5lcg==";
        AccessTokenUser token_original = (AccessTokenUser) fromString(token_ser_b64);
        System.out.println("User: " + token_original.getUsername());
        System.out.println("Token: " + token_original.getAccessToken());
    }

    /** Read the object from Base64 string. */
    private static Object fromString( String s ) throws IOException ,
            ClassNotFoundException {
        byte [] data = Base64.getDecoder().decode( s );
        ObjectInputStream ois = new ObjectInputStream(
                new ByteArrayInputStream(  data ) );
        Object o  = ois.readObject();
        ois.close();
        return o;
    }

    /** Write the object to a Base64 string. */
    private static String toString( Serializable o ) throws IOException {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream( baos );
        oos.writeObject( o );
        oos.close();
        return Base64.getEncoder().encodeToString(baos.toByteArray());
    }
}
```
