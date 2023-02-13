# requsetDto와 responseDto를 만들어서 통신
**제네릭을 사용하지않음**
### RequestDto
```java
package simplechatting2.dto;

import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor
public class RequestDto {
	private String resource;
	private String body;
}

```

### ResponseDto

```java
package simplechatting2.dto;

import lombok.AllArgsConstructor;
import lombok.Data;

@AllArgsConstructor
@Data
public class ResponseDto {
	
	private String resource;
	private String status;
	private String body;
	
}

```
