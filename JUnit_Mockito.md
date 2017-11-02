## Junit
Pull in via gradle
```
testCompile 'junit:junit:4.12'
```

## Mockito
Get the Jars https://mvnrepository.com/artifact/org.mockito/mockito-core
```
testCompile 'org.mockito:mockito-core:2.11.0'
```

Make your life easy, add this import to your tests
```java
import static org.mockito.Mockito.*;
```

### Mock Assertions
Found this a good site. covers mcoking and controlling flow with small easy to use examples
http://static.javadoc.io/org.mockito/mockito-core/2.10.0/org/mockito/Mockito.html


### Mock Interface
```java
Mockito.mock( IFoo.class );
```

*Example*
```java
public interface IPrintBuffer {
  public void print(String message);
}

public class Printer {
  IPrintBuffer buffer;
  public Printer( IPrintBuffer buffer ){
    this.buffer = buffer;
  }
  
  public void startPrintJob( List<String> messages ){
    messages.forEach( message ) -> {
      buffer.print( message );
    });
  }
}

public class PrinterTests {
  @Test public void sampleInterfaceTest(){
    List<String> data = Array.asList( "A Message", "Another Message" );
    IPrintBuffer bufferMock = mock( IPrintBuffer.class );
    Printer sut = new Printer( bufferMock );
    
    sut.startPrintJob( data );
    
    verify( bufferMock ).print( "A Message" );
    verify( bufferMock ).print( "Another Message" );
  }
}
```


### Mock Abstract Class
Following implements an abstract class, using the real methods defined and calls the constructor so any variables are initialized
```java 
Mockito.mock(Foo.class, withSettings().useConstructor().defaultAnswer( Mockito.CALLS_REAL_METHODS ));
```

Example
```java
public abstract class MessageQueue {
  List<String> messages = new ArrayList<String>();
  
  public void addMessage( String message ){
    messages.add( message );
  }
  
  public boolean processMessages();
}

public class SystemProcessor {
  MessageQueue queue;
  public SystemProcessor( MessageQueue queue ){
    this.queue = queue;
  }
  
  public void doStuff(){
    String message = makeDatabaseCall();
    queue.addMessage( message );
    
    boolean result = queue.processMessage();
    if( result == true ){
      String jsonResponse = callARestService();
    }
  }
}

public class SystemProcessorTests(){
  @Test public void someAbstractTesting(){
    MessageQueue queue = mock(MessageQueue.class, withSettings().useConstructor().defaultAnswer( CALLS_REAL_METHODS ));
    when( queue ).processMessage().thenReturn( true );
    ... verify something ...
  }
}
```

