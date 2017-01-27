# SIP UAC basic behavior

#### UAS behavior(rfc 3261, section 8.2)
- outside dialog request에 대해서는 method와는 무관하게 처리해야하는 공통적인 규칙이 있다.
-

- request header에 to tag가 존재해야만 한다.
- to tag가 존재하는 request에 대하여, dialog identifier는 계산하고, 이를 기존에 존재하는 dialog들과 비교하여 매칭되는게 있는지 확인한다.
- if) find matching dialog, 이 request는 mid-dialog request이다. 이때에는 outside of dialog request와 동일하게 처리한다.(section 8.2참고)

- if) to tag는 존재하지만, 일치되는 dialog가 없는 경우,
 uas가 뒤졌거나, misroute된 경우이다.

**processing order of steps**  
0) authentication
- **digest access authentication** 참고!
1) method inspection
- if) 지원되지 않는 method,  
  - 405 response를 보낸다.
  - 405 reponse와 함께 Header의 ALLOW field에 사용 가능한 Method list를 추가한다.
- if) 지원되는 method,
  - goto next steps

2) header inspection

**8.2.2.1 To and REQUEST-URI**
- TO : 초기 Request의 Recipient
- REQUEST-URI :

3) Content Processing(section 8.2.3)

4) Applying Extension

5) Processing the Request  

6) Generating Response(section 8.2.6)
- INVITE Method에만 Provisional Response를 사용하고, 나머지 Method들에서는 Final Response를 바로 발행하여라.  
---

#### uac behavior(rfc 3261, section 8.1)
**uac behavior outside of a dialog**  
- INVITE, OPTION이 해당된다.  

**8.1.1 Generating the Request**  
-d


**8.1.3 Processing Response**  
- UAC는 모든 final response에 대해서 알지 못하는 것이더라도 x00 response로 간주하여 처리할 수 있어야한다.
- ex) 431 response를 받게 되면, 400으로 처리해야 한다.
- 또한, 100번 이상의 인식할 수 없는 Provisional Response에 대해서는 183(session progress)로 처리해야 한다.
따라서, **UAC는 100 response와 183 response를 반드시 처리할 수 있어야한다.**  


**8.1.3.4 Processing 3xx Response(keep)**
- 3xx response는 redirection 관련 response.
- UAC는 Response의 Contract field를 사용하여 redirect request를 만들어야한다.


**8.1.3.5 Processing 4xx Response**  
- 4xx Response는 method와는 무관하게 Response에 대해서 특정 handling을 해줘야한다.
- if) 401, 407
  - section 22.2, 22.3에 따라 인증 과정을 거쳐야한다.
