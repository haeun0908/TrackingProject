# TrackingProject

```
https://github.com/jjkkhh123/Tracking_Project.git
```
```
pip install -r requirements.txt
```

## 📱 휴대폰으로 웹페이지에 접속했을 때, 내 휴대폰의 내장 카메라를 통해 영상이 바로 출력되게 만들기
💡 현재는 ngrok과 같은 도구를 사용해 로컬 서버에 외부에서 HTTPS로 접속할 수 있도록 간단히 테스트한 상태

#### 1. index.html 코드 수정
- `<video>` 태그: 브라우저 내에서 실시간 영상 출력
- `getUserMedia()`: 모바일 웹 브라우저가 카메라에 접근하도록 요청
- `playsinline` 속성: iOS에서 전체 화면 전환 없이 재생되도록 설정
- HTTP 필수: 카메라 접근은 HTTPS 환경에서만 허용됨 (ngrok으로 해결)
```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Face Tag Tracker</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
    <h1>실시간 얼굴 태깅 시스템</h1>
    <div style="text-align:center;">
        <!--<img src="/video_feed" width="640" style="border-radius: 10px; border: 2px solid #6c7ae0;" />-->
        <video id="mobileCam" autoplay playsinline width="640" height="480"
               style="border-radius: 10px; border: 2px solid #6c7ae0;"></video>
    </div>
    <script>
        // 모바일 카메라 스트림 연결
        navigator.mediaDevices.getUserMedia({ video: true })
            .then(stream => {
                document.getElementById('mobileCam').srcObject = stream;
            })
            .catch(error => {
                console.error("카메라 접근 오류:", error);
                alert("카메라 권한을 허용해야 영상을 볼 수 있습니다!");
            });
    </script>
    <div style="text-align: center; margin-top: 10px;">
        <a href="/logout" class="logout-btn">로그아웃</a>
        <button id="ocrBtn" class="ocr-btn">글자 추출</button>
    </div>
    <h2 style="text-align:center;">태그 대기중인 얼굴들</h2>
    <div id="pending" class="pending-container"></div>
    <script src="{{ url_for('static', filename='main.js') }}"></script>
</body>
</html>
```

#### 2. ngrok 설치
먼저 터미널에서 아래 명령으로 ngrok을 설치
```
pip install pyngrok
```
![ngrok 설치]()

#### 3. ngrok 홈페이지에 로그인 (계정이  없으면 먼저 가입)
https://dashboard.ngrok.com/get-started/setup/windows

#### 4. Chocolatey를 통해 ngrok를 설치
💡 대부분의 경우 ngrok을 정상적으로 사용하려면 명령 프롬프트를 '관리자 권한'으로 실행
```
choco install ngrok
```
![Chocolatey를 통해 ngrok를 설치]()

#### 5. 기본 ngrok.yml 구성 파일에 authtoken을 추가
💡 대부분의 경우 ngrok을 정상적으로 사용하려면 명령 프롬프트를 '관리자 권한'으로 실행<br><br>
![authtoken을 추가1]()<br>
“Your Authtoken” 부분에 토큰이 표시. 이걸 복사해서...<br>
```
ngrok config add-authtoken YOUR_AUTHTOKEN
```
여기서 YOUR_AUTHTOKEN 부분은 복사한 토큰으로 변경<br>
![authtoken을 추가2]()

## 실행 방법
💡 Flask와 ngrok은 동시에 실행돼야 함

#### 1. Flask 서버 먼저 실행
```
python main.py
```
![Flask 서버 먼저 실행]()

#### 2. ngrok CLI 버전으로 실행
💡 명령 프롬프트를 새로 키고 아래 명령어 실행 (가상 환경 X)
```
ngrok http 5000
```
![ngrok CLI 버전으로 실행]()

#### 3. 스마트폰에서 아래 주소를 브라우저에 입력
![스마트폰에서 아래 주소를 브라우저에 입력]()

## 🛡️ 에러가 발생한다면
Windows 보안 → 바이러스 및 위협 방지 → “설정 관리” → “제외 추가 또는 제거”에서 ngrok.exe를 예외 목록에 추가
```
C:\ProgramData\chocolatey\lib\ngrok\tools\ngrok.exe
```
![에러가 발생한다면]()
