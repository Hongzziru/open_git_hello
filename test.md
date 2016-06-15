

2016.06.15 수

* /home/alpha/bin/SFProtocol 
 * WRMS 내 protocol.
 * BUS, GW 와 통신하는 기본 프로토콜
 * SFP_CH_S/U/I/T  이게 의미하는 것은?
* /home/alpha/log/mo/gw/MGC#204.log
 * DB를 통해 id와 password를 가져오는 것은 확인
 
  ```shell
  8861 command: /usr/bin/telnet
  68862 args: ['/usr/bin/telnet', '60.30.32.21', '23']
  68863 patterns:
  68864     login:
  68865 buffer (last 100 chars):
  68866 before (last 100 chars): Trying 60.30.32.21...^M^M
  68867
  68868 after: pexpect.TIMEOUT
  ```
 * 위 실제 ip에 접근을 못하는 것으로 추정.

내일 할 것
 * WRMS 서버 프로세스 파악하기
 * 가상의 머신을 만들어 통신 시도하기
 * 서버에서 명령어 받는 부분 찾기
 * 명령어 파싱 후 프로세스 분석
 
