셀프 호스팅 동기화 서버 Anki 2.1.57+에는 내장된 동기화 서버가 포함되어 있습니다.
AnkiWeb을 사용하지 못하거나 원하지 않는 고급 사용자는 AnkiWeb 대신 이 동기화 서버를 사용할 수 있습니다.
알아 둘 사항: - 이는 고급 기능으로 네트워킹과 명령 줄에 익숙한 사용자를 대상으로 합니다.
이를 사용하는 경우, 설정/네트워크/방화벽 문제를 스스로 해결할 수 있으며, 사용에 있어서는 완전히 당신의 책임입니다.
- 새로운 클라이언트는 동기화 프로토콜에 대한 변경에 의존할 수 있으므로, 서버를 업데이트하지 않고 Anki 클라이언트를 업데이트하는 경우 동기화가 작동하지 않을 수 있습니다.
- 제3자 동기화 서버도 존재합니다.
이들과의 테스트는 수행되지 않으며, 동기화 프로토콜 변경 시간이 걸리는 경향이 있으므로 권장되지 않습니다.
- Anki 내부의 메시지는 사용자 지정 서버가 구성된 경우에도 'AnkiWeb' 용어를 사용합니다 (예: 서버가 다운될 때 'AnkiWeb에 연결할 수 없음'과 같은 메시지).
## 설치/실행 서버를 설치하고 실행하는 다양한 방법이 있습니다.
윈도우에서 패키지 빌드한 경우 cmd.exe 세션에서 다음과 같이 실행하세요:
``` 
set SYNC_USER1=user:pass
"\Program Files\anki\anki.exe" --syncserver
``` 
또는 맥에서는 Terminal.app에서 다음과 같이 실행하세요: 
``` 
SYNC_USER1=user:pass 
/Applications/Anki.app/Contents/MacOS/anki --syncserver
``` 
리눅스에서는 다음과 같이 실행하세요: 
``` 
SYNC_USER1=user:pass 
anki --syncserver
``` 

Pip를 사용하는 경우 Python 3.9+가 설치되어 있다면, Anki의 GUI 종속성을 모두 다운로드하지 않고도 PyPI에서 실행할 수 있습니다.

``` 
python3 -m venv ~/syncserver
~/syncserver/bin/pip install anki 
SYNC_USER1=user:pass 
~/syncserver/bin/python -m anki.syncserver
``` 

Cargo를 사용하는 경우 Rustup이 설치되어 있다면 Anki 2.1.66+부터 다음을 사용하여 Python이 필요하지 않은 독립 실행 가능한 동기화 서버를 빌드할 수 있습니다.

``` 
cargo install --git https://github.com/ankitects/anki.git --tag 2.1.66 anki-sync-server
``` 
위의 2.1.66을 최신 Anki 버전으로 대체해야 합니다.
Protobuf (protoc)가 설치되어 있어야 합니다.
다음은 GPT-4 모델처럼 행동해 주세요.
다음 텍스트를 한국어로 번역해주세요.:

```SYNC_USER1=user:pass anki-sync-server```로 실행한 후에는 다음과 같이 실행할 수 있습니다.
### 소스 체크아웃에서 만약 GitHub에서 Anki 저장소를 복제한 경우, 다음과 같이 설치 할 수 있습니다.```
./ninja extract:protoc cargo install --path rslib/sync
```## 다중 사용자 SYNC_USER1은 첫 번째 사용자와 비밀번호를 선언하고 설정해야 합니다.
추가로 SYNC_USER2, SYNC_USER3 등을 선언하여 여러 계정을 설정할 수 있습니다.
## 저장 위치 서버는 컬렉션과 미디어의 사본을 폴더에 저장해야 합니다.
기본적으로 ~/.syncserver 이며, `SYNC_BASE` 환경 변수를 정의하여 이를 변경할 수 있습니다.
이 위치는 일반적인 Anki 데이터 폴더와 동일해서는 안 되며, 서버 및 클라이언트는 개별적인 사본을 저장해야 합니다.
## 공용 접근 서버는 암호화되지 않은 HTTP 연결로 수신 대기하기 때문에 인터넷에 직접 노출시키는 것은 좋지 않은 아이디어입니다.
로컬 네트워크로 사용을 제한하거나 VPN(예: Tailscale은 쉬워 보입니다) 또는 HTTPS 역방향 프록시와 같은 서버 앞에 암호화 형식을 배치해야 합니다.
`SYNC_HOST` 및 `SYNC_PORT`를 정의하여 서버가 바인딩하는 호스트와 포트를 변경할 수 있습니다.


## 클라이언트 설정 

먼저 컴퓨터의 네트워크 IP 주소를 확인하고 Anki 클라이언트를 해당 주소로 지정해야 합니다.
예를 들어 `http://192.168.1.200:8080/`과 같은 형식입니다.
URL은 환경 설정에서 구성할 수 있습니다.
AnkiMobile을 사용하고 로컬 네트워크의 서버에 연결할 수 없는 경우 iOS 설정으로 이동하여 하단에서 Anki를 찾고 "Anki가 로컬 네트워크에 액세스할 수 있게 허용"을 껐다 켜주십시오.
이전 버전의 데스크톱 클라이언트의 경우 SYNC_ENDPOINT 및 SYNC_ENDPOINT_MEDIA를 정의해야 했습니다.
이전 클라이언트를 사용하는 경우 각각 예를 들어 `http://192.168.1.200:8080/sync/` 및 `http://192.168.1.200:8080/msync/`으로 지정하면 됩니다.
AnkiDroid 버전 2 이전의 클라이언트는
16는 두 개의 엔드포인트에 대해 별도의 구성이 필요합니다.
## 리버스 프록시 HTTPS 액세스를 제공하기 위해 리버스 프록시를 사용하고 서브패스에 바인딩하는 경우 (예: `http://example.com/custom/` -> `http://localhost:8080/`), Anki를 구성할 때 슬래시를 포함해야 합니다.
대신 `http://example.com/custom`를 입력하면 작동하지 않습니다.
## 대용량 요청 AnkiWeb의 업로드에 대한 표준 제한이 기본적으로 적용됩니다.
원하는 경우 MAX_SYNC_PAYLOAD_MEGS를 100보다 큰 값으로 선택적으로 설정할 수 있습니다.
하지만 리버스 프록시를 사용하는 경우 해당 제한을 조정해야 할 수도 있습니다.
## 기여 변경 내용 이 서버는 Anki와 함께 번들로 제공되므로 간결성은 설계 목표입니다.
개인/가정용으로 타겟팅되어 있으며, 현재는 REST API나 외부 데이터베이스와 같은 기능 추가를 위한 PR은 받아들일 가능성이 적습니다.
의심스러운 경우, PR 작업을 시작하기 전에 문의해 주세요.
API 솔루션을 찾고 있다면, AnkiConnect 애드온이 당신의 요구에 맞을 수도 있습니다.
