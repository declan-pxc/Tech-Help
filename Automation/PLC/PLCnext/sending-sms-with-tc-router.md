# Sending SMS with TC Router from PLCnext

The TC Router can be powerfully managed through XML communication. PLCnext has the tools for this: `TLS_SOCKET`, `TLS_SEND` and `TLS_RECEIVE`.

## TC Router Configuration

1. Log into the web management.
2. Enable the socket server: _Device Services > Socket Server > Configuration_
3. Enable SMS forward[ing]: _Wireless Network > SMS Configuration_

## PLCnext Configuration
This [library](https://www.plcnextstore.com/world/app/143) can be used to simplify development. If you want to implement, use the following steps.
1. When an SMS is to be sent, create a `TLS_SOCKET` with the TC Router
2. Once established, the string (SMS) to be sent, needs to be converted to a byte buffer. This can be done using `STRING_TO_BUF`. You will need to create a byte array to handle this. Keep in mind, a byte is used per character so the size of the array will need to be large to handle the wrapper as well around the message.
3. An example of the string that will be sent: `<?xml version="1.0"?><cmgs destaddr="+61412345678">This is an example SMS</cmgs>`. From the TC Router, it requests no line breaks or other characters.
5. Use `TLS_SEND` to communicate with the TC Router.
6. Then trigger the `TLS_RECEIVE` block (ensure to update the `dwHandle` with the `TLS_SOCKET.dwHandle` as it can change after sending).
7. Once a response is received, it can be converted to a string by using the reverse `BUF_TO_STRING`. It should look like this:

```
<?xml version="1.0"?>
<result>
  <cmgs length="46">SMS Transmitted</cmgs>
</result>
```
7. Once this is complete, the socket is closed.
