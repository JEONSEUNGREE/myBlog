---
title: Oauth2_DefaultOAuth2UserService
tags:
  - SpringSecurity
categories:
  - SpringSecurity
date: 2021-10-19 11:49:22
---


![oauth2_1](/review_img/oauth2/1.PNG)
기본적으로 oauth2도 다른 세션,jwt와 같이 userDetail서비스에 해당하는 클래스가있어서
resource서버으로부터 받은 데이터로 loadUser에서 값을 처리한다.
![oauth2_2](/review_img/oauth2/2.PNG)
null값 체크후에 문제가 없다면 processOauth2User메서드로 이동하게되고 이 메서드에서 oauth로 받은계정이
이전과 같이다른경우( ex)구글계정프로필이업데이트된경우 ) 그에따른 업데이트를 진행 
![oauth2_3](/review_img/oauth2/4.PNG)
위와같이 각 provider별로 나눠져있다. 제공하는 attribute가 다르기때문에 bs4쓰듯이 값을 찾아서 꺼내야한다.





