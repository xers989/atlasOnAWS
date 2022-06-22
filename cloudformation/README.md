### AWS CloudFormation
CloudFormation 을 이용한 MongoDB Atlas 의 배포 자동화 (Infrastucture as a code)


### MongoDB Atlas API
MongoDB Atlas 의 API 실행을 위한 Key 발행합니다.    
Atlas console 에 로그인 후 상단에 Access Manager 를 클릭 합니디.    
API Keys 탭을 클릭 하고 Create API Key 를 클릭 하여 줍니다.   
설명란에 생성할 API key 의 설명 ("Cloudformation key")을 입력 하여 주고 권한은 Atlas 프로젝트를 생성 할 것 이기 때문에 Project Creator, Orgnization member 를 선택 하여 줍니다.   
<img src="/cloudformation/images/images01.png" width="90%" height="90%">  

다음을 클릭 하면 Public key 와 Private Key 가 생성 됩니다. Private Key 는 다시 조회가 않됨으로 외부 노출이 되지 않게 보관 하여 줍니다. API 호출은 white list 정책에 따라서 호출이 됩니다. 테스트를 위해서 전체 IP 가 허용 되도록 하지만 cloudformation 이 수행 되는 AWS 리전의 IP 를 확인 하고 등록 하여 줍니다.
<img src="/cloudformation/images/images02.png" width="90%" height="90%">  

추가로 Atlas 의 Organization code 가 필요 합니다. 상단의 organization 을 클릭 하고 Settings 를 보면 Orgnization ID 를 확인 할 수 있습니다.
<img src="/cloudformation/images/images03.png" width="90%" height="90%">  


### AWS Cloudformation
AWS console 에 로그인 후 리전을 선택한 후 cloudformation 을 실행 합니다. (데모에서는 서울 리전을 사용 합니다.)    
Stacks를 클릭 한 후 Create Stack을 클릭 합니다 (with new resources 선택).  
현재 Template 는 Atlas Cluster 를 생성 하기 및 Peering 을 포함한 생성 등을 제공 하고 있습니다. 데모에서는 VPC peering을 포함하지 않는 단독 MongoDB 배포를 진행합니다.   
템플릿 주소는 다음과 같습니다.   
https://aws-quickstart.s3.amazonaws.com/quickstart-mongodb-atlas/templates/mongodb-atlas-main.template.yaml
  
<img src="/cloudformation/images/images04.png" width="90%" height="90%">  
  
다음 화면에서 Atlas API 를 수행 하기 위한 Key 정보를 입력합니다. 이전에 생성 했던 public key, private key, organization id 를 입력 하여 줍니다. 선택 사항으로 Atlas project 이름과 MongoDB 버전, 클러스터의 이름, 생성할 리전 정보, Atlas 의 클러스터 크기를 지정 할 수 있습니다.
<img src="/cloudformation/images/images05.png" width="90%" height="90%">  

데모에서는 M10 을 선택 하고 배포 지역은 서울 (ap-northeast-2) 를 선택 하여 배포 진행 합니다.   

마지막 스택의 옵션을 확인 하고 다음을 선택 합니다. 전체 옵션을 다시 한번 확인 후 Capabilities 를 선택 하고 Create stack 을 클릭 합니다.   

<img src="/cloudformation/images/images06.png" width="90%" height="90%">  
배포는 약 6-7 분 가량이 소요 됩니다.

배포가 완료 되면 다음과 같이 배포가 성공했다는 메시지를 볼 수 있습니다.
<img src="/cloudformation/images/images07.png" width="90%" height="90%">  


### Atlas Console 
MongoDB Atlas 에 로그인 하면 다음과 같이 Database 가 생성 된 것을 확인 할 수 있습니다.
입력 하여준 project 가 생성 되고 해당 프로젝트 하위로 Cluster 가 생성 됩니다.
<img src="/cloudformation/images/images08.png" width="90%" height="90%">  