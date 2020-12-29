# AWS Lambda 활용 
- Legacy :   
기존 시스템은 인프라부터 소프트웨어까지 전부 구축하고 개발해야 합니다.  
- Infrastructure-as-a-Service (IaaS):  
필요한 하드웨어와 가상화, OS 등 인프라 요소를 서비스 형태로 제공합니다. 원하는 사양의 서버를 VM 으로 생성할 수 있습니다.  
- Container-as-a-Service (CaaS):   
서비스 형태로 제공되는 컨테이너를 활용해 애플리케이션을 배포합니다.  
- Platform-as-a-Service (PaaS):   
애플리케이션 개발에 집중할 수 있도록 인프라와 런타임 환경을 제공합니다. 
- `Function-as-a-Service` (FaaS):   
실행할 함수 코드에만 집중할 수 있습니다.  
- Software-as-a-Service (SaaS):   
제공되는 소프트웨어를 사용하는 형태입니다.  

### Function 내부 구조
> `Event Source` -> `Function` -> `Service`  
- Event Source: 함수가 실행될 조건이자 이벤트 소스 (HTTP 요청, 메시징, Cron 등)  
- Function: 작업할 내용  
- Service: 작업 결과를 처리(DB 저장, 다른 서비스로 전달, 메시징, 출력 등)  

### FaaS 성능 최적화 (AWS Lambda 함수의 라이프사이클)
> 처음에 해당 함수 코드를 찾아 다운로드하고 새로운 실행 환경을 구성합니다.   
> 이 과정을 차갑게 식은 서버를 실행하는 것에 비유해 콜드 스타트(Cold Start)라고 합니다.  
> 함수를 처음 호출할 때나 업데이트 된 후 실행할 경우 어쩔 수 없이 발생하는 지연(delay)입니다.
- 해당 서버가 아직 내려가지 않은 따뜻한(warm) 상태라면 준비 과정(Cold Start)을 거치지 않고 빠르게 함수가 수행됩니다. 
이를 이용해 주기적으로 함수를 호출하도록 스케줄링하면, 서버가 내려가지 않도록 warm 상태를 유지하게 됩니다.

### 일반적인 웹 애플리케이션을 서버리스 형태로 구성한 아키텍처
- 사용자에게 보여줄 웹 페이지 및 정적 콘텐츠는 S3 에 저장 후 호스팅  
- 사용자 요청은 API Gateway 로 받기  
- 처리할 내용은 Lambda 에 작성  
- 데이터 저장은 DB 서비스(DynamoDB) 사용  
- 사용자 인증은 Amazon Cognito 사용  
- Route 53으로 도메인 구입 및 제공  