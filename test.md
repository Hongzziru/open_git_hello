2016.06.29 수요일

* 분당 회의
  * gw - simulator 접속 끊어지던 현상 완료. 
    1. 관리하지 않는 포트 번호를 사용함으로써 발생
    2. mps, simulator 프로세스를 중복으로 띄움으로써 발생
  * 수정한 코드 원상 복귀 필요
  * simulator - gw 포트를 이어준 후  GW에 telnet으로 붙어 cod/pod 확인 필요
  
