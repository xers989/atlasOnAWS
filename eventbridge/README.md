### AWS Eventbridge 를 이용한 MongoDB Ingesting
MongoDB Atlas 에 생성 되는 데이터를 AWS EvetBridge 에 연결 하여 데이터를 수집 합니다. 수집한 데이터는 다양한 방법으로 사용이 가능하며 이번 데모에서는 간단히 Cloudwatch에서 데이터를 볼 수 있도록 로깅을 합니다.
(AWS 의 MongoDB 활용 블로그 참고 : https://aws.amazon.com/blogs/compute/ingesting-mongodb-atlas-data-using-amazon-eventbridge/)

### MongoDB Atlas Trigger
테스트를 위해 데이터를 생성하여 줍니다. 클러스터 메뉴에서 load sample data 를 선택 하면 샘플 데이터가 생성 됩니다. (약 10분 소요)    
<img src="/eventbridge/images/images01.png" width="90%" height="90%">

Sample_analytics 데이터베이스에 있는 customers 컬렉션에 데이터 변경이 발생을 감지하고 이를 ingesting 하는 것으로 Atlas 는 trigger 를 이용하여 이벤화 하여 전달 합니다. 이를 위해 trigger 를 생성 하여 주고 이를 소비할 대상 (EventBridge)를 설정 하여 줍니다.    


데이터 생성이 완료 되면 Data services 메뉴 중 triggers 를 선택 합니다.    
<img src="/eventbridge/images/images02.png" width="90%" height="90%">

Add new trigger 를 클릭 하여 trigger 이름을 입력 하여 주고 trigger source 에서 cluster name (생성한 클러스터를 선택)과 database name(sample_analytics) 을 선택 하여 줍니다.   
Trigger 대상 컬렉션은 customers로 하며 이벤트는 insert, update, delete, replace 를 모두 선택 합니다.   
<img src="/eventbridge/images/images03.png" width="90%" height="90%">

마지막으로 function 부분은 EventBridge를 선택 합니다.   
입력할 정보는 AWS Account ID 와 EventBridge 가 있는 리전 정보 입니다. (데옴에서 Tokyo를 선택 합니)
<img src="/eventbridge/images/images04.png" width="90%" height="90%">


### AWS EventBridge
생성한 EventBridge 정보는 Pending 상태로 AWS 에 등록 대기 상태로 존재 합니다. Eventbridge 설정을 위해 AWS console 에 로그인 후 EventBridge를 실행합니다.    
EventBridge 의 Partner event sources 를 보면 다음과 같이 pending 상태로 있는 소스를 볼 수 있으며 Associate with event bus 를 클릭 하여 줍니다.    
<img src="/eventbridge/images/images05.png" width="90%" height="90%">

cloudformation으로 필요한 AWS 리소스를 배포하는 template 이 있음으로 이를 실행 하여 줍니다. 실행는 다음 command 로 수행 하여 줍니다.

````
$ sam deploy --guided
````

실행을 위해서는 awscli 등의 설치가 필요합니다.   
실행과정에서 eventBridge source 에 입력에서 생성된 event source 를 입력 하여 줍니다.

(Partner Event Sources 에서 이름을 확인 할 수 있습니다.)    
<img src="/eventbridge/images/images06.png" width="90%" height="90%">


### Data 생성
Atlas Console 에 로그인 후 Cluster를 클릭 후 Sample_analytics.customers 컬렉션을 선택하여 줍니다.    
<img src="/eventbridge/images/images07.png" width="90%" height="90%">    

insert document 버튼을 클릭 하여 view 를 JSON 으로 선택 한 후 다음을 Json 메시지를 입력 하여 줍니다.

<img src="/eventbridge/images/images08.png" width="90%" height="90%">    

````
{
    "username":"atlas collection insert",
  "name":"Eventbridge Mongo",
  "address":"My Address XYZ",
  "birthdate":{"$date":"1975-03-02T02:20:31.000Z"},
  "email":"mygmailid@gmail.com",
  "active":true,
  "accounts":[371138,324287,276528,332179,422649,387979],
  "tier_and_details":{
     "0df078f33aa74a2e9696e0520c1a828a":{
     "tier":"Bronze",
     "id":"0df078f33aa74a2e9696e0520c1a828a",
     "active":true,
     "benefits":["sports tickets"]
    },
   "699456451cc24f028d2aa99d7534c219":{
   "tier":"Bronze",
   "benefits":["24 hour dedicated line","concierge services"],
   "active":true,
   "id":"699456451cc24f028d2aa99d7534c219"
  }
  }
}

````

<img src="/eventbridge/images/images09.png" width="90%" height="90%">   

Insert를 클릭 하면 데이터가 생성 됩니다.   

AWS 에 로그인 후 Cloudwatch 를 실행 합니다.

Log groups 에서 /aws/lambda/myeventfunction 을 선택 합니다.

<img src="/eventbridge/images/images10.png" width="90%" height="90%">   

로그 정보에서 MongoDB 에 생성된 정보가 전달 된 것을 확인 할 수 있습니다.   


<img src="/eventbridge/images/images11.png" width="90%" height="90%">   