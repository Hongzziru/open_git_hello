
2016.06.16 목요일

서버 프로세스 
 1. /home/alpha/bin/cmdExecute-LG-MGC-1_ 실행.
 2. DB DCC_COMMAND, DCC_STATE 테이블을 통해 현재 상태 확인 및 생성된 명령어를 가져온다.
 3. cmdExecute는 cannel-GW(CG-SIO-TCPD-TCPC)로 절체 명령어를 전달한다.
 4. cannel-GW는 GW(COD-GW)로 명령어를 전달한다.
 5. GW는 실제 장비에 명령어를 전달한다.
 6. GW는 실제 장비에서 recv 된 정보를 cannel-GW로 전달하고 cannel-GW는 recv를 파싱하여 PA-SI-WRITE-TCPC로 보낸다.
 7. PA-SI-WRITE-TCPC는 받은 정보를 버스로 전달한다.
 8. 버스는 정보를 UI, CA-TCPC-SO(CA-)로 보낸다. CA-는 DB로 파싱 과정을 거쳐 저장하게 된다.
 
내일 할 것
 * 각 프로세스 log 찾기
 * 명령어 전달 시도 - /home/alpha/bin/cmdExecute-LG...  
 * 명령어 던진 후 과정 log로 확인 필요
