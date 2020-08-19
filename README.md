AnipleAndroid
===============
#### 사용자 주변 동물 병원, 약국, 미용샵, 용품점을 검색하고 장소 정보, 반려동물 관련 이벤트 등을 보여주는 어플리케이션 
> 개발 기간: 2020.07.06 ~ 2020.08.28

> 개발 환경 & 언어 : 안드로이드 스튜디오 & kotlin
<p float="left">
<img width="200" height="300" alt="스크린샷 2020-08-04 오후 2 59 22" src="https://user-images.githubusercontent.com/50612841/89860440-690e7800-dbde-11ea-8959-61fb45518dca.png">
<img width="200" height="300" alt="스크린샷 2020-08-04 오후 2 59 01" src="https://user-images.githubusercontent.com/50612841/89964748-f6f16e00-dc85-11ea-8263-e24023f11785.png">
<img width="200" height="300" alt="스크린샷 2020-08-04 오후 3 03 51" src="https://user-images.githubusercontent.com/50612841/89860453-71ff4980-dbde-11ea-9933-7c3813f43f21.png">
<img width="200" height="300" alt="스크린샷 2020-08-04 오후 2 59 38" src="https://user-images.githubusercontent.com/50612841/89860485-87747380-dbde-11ea-90e9-494ab537173a.png">
</p>

## 1. 구현 과정 
### 1.1 1주차 
kotlin 공부 & 프로젝트 파악(필요한 기능 공부)
### 1.2 2주차 
zeplin으로 디자인 리소스 공유 받고 레이아웃 생성 & 네이버지도 api 연결
### 1.3 3주차 
서버 api 연결 & 레이아웃 생성 
### 1.4 4주차
glide등 라이브러리 적용 & 재검색 기능 추가 구현 
### 1.5 5주차 
네크워크 예외 처리 & 버그 해결 & fcm 
### 1.6 6주차 
구글 광고 & 코드 리팩토링 
### 1.7 7주차
지도 줌레벨에 따라 유동적으로 검색 범위 바뀌게끔 수정 & 알파테스트 
### 1.8 8주차 
진행중 

## 2. 발생 이슈

### 2.1 네이버 지도 현재위치 로드시에 카메라 이동 이슈 
지도에 사용자 현재위치를 표현하기 위해 네이버 지도 개발가이드를 참고하여 FusedLocationSource와 네이버 sdk자체에서 제공하는 위치추적 모드로 사용자 현재위치를 구현하였다. 맨 처음 지도를 키면 사용자위치에서 화면이 시작하여 현재위치가 보여지는 것이 아닌 서울시청(네이버 sdk에 지정된 기본 위치)에서 현재위치로 카메라 이동이 일어나는 이슈가 발생하였다. 지도가 로드된 후(지도 객체가 얻어진 후) 카메라 위치 설정 없이 위치 추적 모드를 실행하기 때문에 발생한 이슈였다. 이를 해결하기 위해 onMapReady가 호출되는 시점에서 FusedLocationProviderClient를 사용하여 사용자 현재 좌표를 얻고 지도자체 카메라 위치를 현재위치로 설정해주었고 지도 시작시에 카메라 이동 없이 바로 사용자 현재위치로 화면을 잡을 수 있었다. 

### 2.2 app kill 상태시 fcm 알람이 오지 않는 이슈 

![image](https://user-images.githubusercontent.com/50612841/89861682-50ec2800-dbe1-11ea-9a28-d8d1e221bdd9.png)
fcm test과정에서 포그라운드랑 백그라운드 상태에서는 푸쉬알람이 제대로 오는것을 확인하였으나, 앱이 완전이 죽은 상태에서는 알람이 수신되지 않는 이슈가 발생하였다. 구글링을 통해 찾은 여러 방법을 모두 시도해보았지만
푸쉬알람이 와도 바로 오지않고 30분 뒤에 몰아서 오는 등 제대로 작동하지 않았다. 스택오버플로우랑 깃허브 이슈를 뒤져본 결과 위와 같은 해결법을 찾을 수 있었다. 정확한 이유는 모르겠으나 앱을 실행할때 run으로 실행시키지 않고
핸드폰에 깔려있는 런처 아이콘을 통해 실행시키고 테스트를 진행하니 제대로 동작하였다. 아마도 os 또는 핸드폰 기종 문제 같다.(이슈관련 글을 보니 어떤 핸드폰은 위와 같은 방법도 먹히지 않는다는 댓글을 보았다.)
확실히 작동되는 것을 확인하기 위해 릴리즈 버전으로 앱번들을 만들고 apks를 깔아서 테스트 해보니 제대로 잘 동작하였다. 

## 3. 배운점 
java로 앱개발을 할 당시에는 xml에 있는 view들을 일일이 다 선언해서 써야 했는데 kotlin으로 개발하면서 코드를 훨씬 간결하게 만들 수 있었다. activity 호출 이라던지 공통된 부분은 activity를 하나 만들어 그 안에 넣고 모든 activity가 해당 activity를 상속하도록 만들었다. 이렇게 하니 훨씬 더 코드를 간결하고 가독성 좋게 만들 수 있었다. 

클래스 파일들도 도메인별로 구분해서 폴더화 시켜놓았다. 함수 하나 찾으려면 오래걸리던 전과 달리 저렇게 구분해놓으니 훨씬 찾기 좋아졌다. 폴더화를 습관화시켜야겠다는 생각이 들었다. 애니플 프로젝트를 하기전 진행했던 프로젝트에서는 아파치에서 기본으로 제공하는 http client를 사용하여 api 연결 부분을 구현하였는데 헤더나 파라미터를 넣으려면 꽤 긴 코드를 작성해야했다.

retrofit을 사용하니 전에는 몇십줄 되던게 최대 10줄 정도로 줄었다. 또한 단순히 이미지 뷰만 사용하던 전과 달리 이번 프로젝트에서는 이미지 라이브러리인 glide를 사용하였는데 이것또한 굉장히 편리한 기능을 제공하는 라이브러리였다. 네이버 오브젝트 스토리지로 이미지를 다운받았을적에는 이미지 다운 구현하는데에 굉장히 많은 시간이 들었지만 glide는 단순히 이미지 url만 넣어주면 알아서 이미지뷰에 넣어주므로 그런 수고를 할 필요가 없었다. 그밖에도 테두리 조절이라던지 썸네일이라던지 오류시에 보여지는 화면등도 코드한줄이면 구현할 수 있었다. 

어플을 개발하면서 이미지 사이즈가 커서 느리게 로드 되는 등 나는 당연하게 여기는 문제들이 사용자가 어플리케이션 사용시에는  오류로 느낄 수 있다는 것을 배웠다 . 이런 경우에는 로딩화면을 만드는 등 사용자가 지루하지 않게 해주어야 한다. 평소에는 UX적인 관점에서 잘 생각해보지 않았는데 UI자체 말고도 UX도 고민해야할 부분이 많다는것을 깨달았다.
