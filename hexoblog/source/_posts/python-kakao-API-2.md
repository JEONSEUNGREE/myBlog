---
title: python_kakao_API_2(카톡받기))
tags:
  - Python
categories:
  - Python
date: 2021-09-29 22:18:12
---

1) 카카오개발자 사이트접속 애플리케이션등록
2) 사용하고자하는 애플리케이션 선택

카톡메시지를 받기위한 요청보내기
![python_1](/review_img/KakaoAPI/6.PNG)

다음과같이 요청을 보낸다.
![python_2](/review_img/KakaoAPI2/1.PNG)

로그인 요청
![python_3](/review_img/KakaoAPI2/2.PNG)

그러면 동의화면 출력후 다음으로 넘어간다(시간이걸림)

이런식으로 리다이렉트 되면서 특정 코드값을 보내준다.(일정 시간 내에 해야됨)
https://localhost.com/?code={code}

토큰, api키는 노출되지않도록 주의 (다음과 같이 요청시 액세스토큰, 리프레쉬토큰을 보내준다.)
![python_4](/review_img/KakaoAPI2/3.PNG)

토큰 파일로 저장하는 과정과 리프레쉬 과정
![python_5](/review_img/KakaoAPI2/4.PNG)

저장된 파일
![python_6](/review_img/KakaoAPI2/6.PNG)

최종적으로 메시지를 받기위한 과정
![python_7](/review_img/KakaoAPI2/5.PNG)

최종적으로 받은 알림
![python_8](/review_img/KakaoAPI2/7.PNG)

