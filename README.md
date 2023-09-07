Techlabs RTB Exchange 연동 가이드
=======================

* [1. Techlabs 소개](#1-Techlabs-소개)
    * [1.1 Techlabs RTB Exchange](#11-Techlabs-rtb)
    * [1.2 Techlabs 연동 절차](#12-Techlabs-연동-절차)
    * [1.3 Techlabs 지원 배너 종류](#13-Techlabs-지원-배너-종류)
* [2. OpenRTB Basics](#2-openrtb-basics)
    * [2.1 입찰 요청](#21-입찰-요청)
    * [2.2 입찰 요청에 대한 응답](#22-입찰-요청에-대한-응답)
* [3. 입찰 요청(Bid Request Specification)](#3-입찰-요청bid-request-specification)
    * [3.1 Object: BidRequest](#31-object-bidrequest)
    * [3.2 Object: Imp](#32-object-imp)
    * [3.3 Object: Banner](#33-object-banner)
    * [3.4 Object: Native](#34-object-native)
    * [3.5 Object: Site](#35-object-site)
    * [3.6 Object: App](#36-object-app)
    * [3.7 Object: Device](#37-object-device)
    * [3.7.1 Object : Geo](#371-object--geo)
    * [3.8 Object: User](#38-object-user)
    * [3.9 Object: Video](#39-object-video)
* [4. 입찰 응답(Bid Response Specification)](#4-입찰-응답bid-response-specification)
    * [4.1 Object: BidResponse](#41-object-bidresponse)
    * [4.2 Object: SeatBid](#42-object-seatbid)
    * [4.3 Object: Bid](#43-object-bid)
    * [4.4 매크로 치환 (Substitution Macros)](#44-매크로-치환-Substitution-Macros)
* [5. Native 규격](#5-native-규격)
    * [5.1 입찰 요청](#51-입찰-요청)
        * [5.1.1 Native Markup Request Object](#511-Native-Markup-Request-Object)
        * [5.1.2 Asset Request Object](#512-Asset-Request-Object)
            * [5.1.2.1 Title Request Object](#5121-Title-Request-Object)
            * [5.1.2.2 Image Request Object](#5122-Image-Request-Object)
            * [5.1.2.3 Data Request Object](#5123-Data-Request-Object)
    * [5.2 입찰 응답](#52-입찰-응답)
        * [5.2.1 Native Markup Response Object](#521-Native-Markup-Response-Object)
* [6. 비딩 입찰 요청 예제(Bid Request Samples)](#6-비딩-입찰-요청-예제bid-request-response-samples)
    * [6.1 Bid Requests](#61-bid-requests)
        * [6.1.1 Example - 디스플레이 광고 요청](#611-example---디스플레이-광고-요청)
        * [6.1.2 Example - Native 광고 요청](#612-example---Native-광고-요청)
        * [6.1.3 Example - Video 광고 요청](#613-example---Video-광고-요청)
* [7. 쿠키교환 Cookie Matching - Cookie Sync](#7-쿠키교환-cookie-matching---cookie-sync)
    * [7.1 사용자 ID 교환 요청 방식](#71-사용자-ID-교환-요청-방식)
      <br/><br/>

# 1. Techlabs 소개

## 1.1 Techlabs RTB Exchange (DSP)

* 본 문서에서는, Techlabs Network사 간의 OpenRTB 프로토콜을 통해 연동하는 방법을 안내합니다.
* OpenRTB는 2010년 부터 IAB(Interactive Advertising Bureau)에서 제작되어 현재 까지 개발 진행 중입니다.
* Techlabs ***IAB OpenRTB*** 을 기반으로 합니다.
    * IAB OpenRTB API 2.5
      https://iabtechlab.com/standards/openrtb/
    * IAB OpenRTB Dynamic Native Ads API 1.2
      https://iabtechlab.com/standards/openrtb-native/
    * IAB Video Ad Serving Template(VAST) 3.0
      https://iabtechlab.com/standards/vast/
* 단, 모든 스펙이 구현되어 있지는 않으며, 연동시 추가로 필요한 스펙은 양사간 협의하에 추가 구현됩니다.

## 1.2 Techlabs 연동 절차

 순서 | 내용                                          | 담당            
:---|:--------------------------------------------|:--------------
 00 | 문의                                          | Techlabs      
 01 | DSP 연동 가이드 및 questionnaire 전달               | DSP           
 02 | DSP 연동 가이드 검토 및 DSP questionnaire 작성        | Techlabs      
 03 | 가능 여부 판단 후 디바이스 및 사이즈 협의                    | Techlabs, DSP 
 04 | 양사 RTB 스펙 확인 후 예시 요청문을 통한 Techlabs 내부 설정 진행 | Techlabs, DSP 
 05 | 계약서 전달 및 날인                                 | Techlabs, DSP 
 06 | DSP의 사용자 식별값과 Techlabs의 사용자 식별자 연동          | Techlabs, DSP 
 06 | Test 캠페인 설정 및 EndPoint 및 응답 전문 전달           | Techlabs, DSP 
 07 | 응답 전문 검토 확인                                 | Techlabs      
 08 | 테스트 연동 요청 시작                                | Techlabs, DSP 
 09 | 모니터링                                        | Techlabs, DSP 
 10 | 테스트 종료 및 통계 정보 확인                           | Techlabs, DSP 
 11 | 상용 연동 시작                                    | Techlabs, DSP 

## 1.3 Techlabs 지원 배너 종류

* 디스플레이 배너
    * HTML (iframe or full static html) 을 매체 지면에 노출
* Native 배너
    * OpenRTB Native Spec 에 따라 매체 지면에 랜더링

<br/><br/>

# 2. OpenRTB Basics

[Open RTB 알아보기](https://adop.cc/%EB%94%94%EC%A7%80%ED%84%B8-%EA%B4%91%EA%B3%A0%EC%9D%98-%EA%B7%9C%EC%95%BD-open-rtb-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0/)

## 2.1 입찰 요청 방식

입찰 요청은 DSP부터 부여받은 end-point 로 HTTP POST 프로토콜로 요청합니다.

```
// 요청 헤더 (필수)
Content-Type: application/json
// DSP측에서 요구하는 해더를 추가적으로 적용한다.
```

## 2.2 입찰 응답 처리 방식

* HTTP 코드 200 일 경우 입찰이 되어 광고가 있는것으로 판단
* 이 외의 경우는 패찰로 판단
* Exchange 내부에서 개찰 후 패찰 요청자에게 결과를 알려준다.

<br/><br/>

# 3. 입찰 요청(Bid Request Specification)

Techlabs Exchange 에서 매체사와 클라이언트의 정보를 IAB Protocol로 가공해 DSP로 입찰 요청을 보냅니다.
BidRequest는 하나 이상의 Imp(impression) Object로 구성되며, Imp에 대한 추가 정보를 추가하여 연동합니다.

## 3.1 Object: BidRequest

입찰 최상위 오브젝트

 Name   | Type         | 필수, 기본값     | Description                                             
:-------|:-------------|:------------|:--------------------------------------------------------
 id     | string       | 필수          | 입찰에 대한 유니크 아이디                                          
 imp    | object array | 필수          | imp 오브젝트 배열 (다중 요청 가능)                                  
 site   | object       | 필수(or app)  | app,site 오브젝트 둘 중 하나가 반드시 포함됨                           
 app    | object       | 필수(or site) | app,site 오브젝트 둘 중 하나가 반드시 포함됨                           
 device | object       |             | 광고가 전송될 디바이스 특성, 종류등을 설명합니다.                            
 user   | object       | 필수          | 사용자 정보                                                  
 test   | integer      |             | 1인 경우 테스트 연동                                            
 at     | integer      | 필수          | Auction Type 1: 1st-price auction, 2: 2nd-price auction 
 cur    | string array | 필수          | ISO–4217 코드의 단위통화 리스트. - 계약시 USD, KRW 중 확정 필요           
 bcat   | string array |             | 제외되어야 할 광고주 카테고리 리스트<br/>IAB OpenRTB Spec 2.5 > 표 5.1 조 
 badv   | string array |             | 제외되어야 할 광고주의 최상위 도메인 리스트.                               
 ext   | object       |             | 기타정보                               


## 3.2 Object: Imp

입찰 대상이 되는 광고의 위치나 광고 종류 등을 나타냅니다.

 Name        | Type    | 필수, 기본값 | Description                               
:------------|:--------|:--------|:------------------------------------------
 id          | string  | 필수      | BidRequest 오브젝트 안에서 imp를 구분하기 위한 고유 식별자   
 banner      | object  | 필수      | banner, native 오브젝트중 하나 이상을 포함하고 있어야 합니다. 
 native      | object  | 필수      | banner, native 오브젝트중 하나 이상을 포함하고 있어야 합니다. 
 instl       | integer |         | 전면광고 여부. 1일 경우 전면                         
 tagid       | string  | 필수      | 노출 인벤토리(해당 지면, 유닛)의 고유한 식별자               
 bidfloor    | integer |         | Impression의 입찰 최저가                        
 bidfloorcur | string  | 필수      | 계약시 USD, KRW 중 확정 필요                      

## 3.3 Object: Banner

디스플레이 광고. native 가 아닌 일반 광고일 경우 반드시 포함되어야 합니다.

 Name  | Type          | 필수, 기본값 | Description                                         
:------|:--------------|:--------|:----------------------------------------------------
 w     | integer       | 필수      | 광고의 넓이(pixel)                                       
 h     | integer       | 필수      | 광고의 높이(pixel)                                       
 btype | integer array |         | 제외 되어야 할 광고소재 종류<br>IAB OpenRTB Spec 2.5 > 표 5.2 참조 
 battr | integer array |         | 제외 되어야 할 광고소재 속성<br>IAB OpenRTB Spec 2.5 > 표 5.3 참조 

## 3.4 Object: Native

Native 형식의 Impression을 나타냅니다. Open RTB Native Spec에 의해 요청된 소재정보를 입찰시 응답 합니다.

 Name    | Type   | 필수, 기본값 | Description                                                             
:--------|:-------|:--------|:------------------------------------------------------------------------
 request | string | 필수      | Native Ad Spec 을 준수하는 요청 페이로드<br>Open RTB Native Spec 에 의거한 Json stirng 
 ver     | string |         | Native Ad Spec 버전                                                       

## 3.5 Object: Site

광고가 전송될 지면이 웹사이트일 경우 반드시 포함되어야 합니다. site오브젝트와 app오브젝트와 동시에 포함 할 수 없습니다.

 Name       | Type         | 필수, 기본값 | Description                          
:-----------|:-------------|:--------|:-------------------------------------
 id         | string       | 필수      | 사이트 ID                               
 name       | string       | 필수      | 사이트 이름                               
 domain     | string       | 필수      | 사이트 도메인. 광고주에서 블럭 처리하는데 사용 할 수 있습니다. 
 cat        | string array |         | 전체 IAB 카테고리 리스트                      
 sectioncat | string array |         | 현재 섹션의 IAB 카테고리 리스트                  
 mobile     | integer      |         | 모바일 최적화 여부 0 = no, 1 = yes           
 publisher  | object       |         | publisher 상세 정보                      
 content    | object       |         | content 상세 정보                        

## 3.6 Object: App

광고가 전송될 지면이 어플리케이션일 경우 반드시 포함되어야 합니다. site오브젝트와 app오브젝트와 동시에 포함 할 수 없습니다.

 Name       | Type         | 필수, 기본값 | Description                            
:-----------|:-------------|:--------|:---------------------------------------
 id         | string       | 필수      | 앱 ID                                   
 name       | string       | 필수      | 앱 이름                                   
 bundle     | string       | 필수      | Android 패키지명, IOS에서는 패키지명 혹은 어플리케이션 ID 
 domain     | string       |         | 어플리케이션 도메인.                            
 storeurl   | string       | 권장      | 앱스토어 URL                               
 cat        | string array |         | 전체 IAB 카테고리 리스트                        
 sectioncat | string array |         | 현재 섹션의 IAB 카테고리 리스트                    
 ver        | integer      |         | 어플리케이션 버전                              
 paid       | integer      |         | 0 = 무료, 1 = 유료                         
 publisher  | object       |         | publisher 상세 정보                        
 content    | object       |         | content 상세 정보                          

## 3.7 Object: Device

하드웨어, 플랫폼, 위치, 통신사등 해당 기기와 관련되 정보를 제공합니다.

 Name           | Type    | 필수, 기본값 | Description                                               
:---------------|:--------|:--------|:----------------------------------------------------------
 ua             | string  | 필수      | User Agent - 지면이 웹인 경우 필수                                 
 geo            | object  |         | 위치 정보                                                     
 ip             | string  | 필수      | IPv4 주소                                                   
 devicetype     | integer |         | 디바이스 종류. IAB OpenRTB Spec 2.5 > 표 5.17 참조                 
 make           | string  |         | 디바이스 제조사                                                  
 model          | string  |         | 디바이스 모델                                                   
 os             | string  | 필수      | 디바이스 운영체제 (android, ios)                                  
 osv            | string  |         | 디바이스 운영체제 버전                                              
 h              | integer |         | 디바이스 넓이(pixel)                                            
 w              | integer |         | 디바이스 높이(pixel)                                            
 language       | string  |         | 브라우저 언어. ISO-639-1-alpha-2                                
 carrier        | string  |         | 통신사 또는 IP 어드레스로 부터 유도된 ISP(인터넷 서비스 제공자)                   
 connectiontype | string  |         | 네트워크 연결 종류 IAB OpenRTB Spec 2.5 > 표 5.18 참조               
 ifa            | string  | 필수      | 광고 트래킹 아이디(ex) android = gaid, ios = idfa) - 지면이 앱인 경우 필수 
추후 적용 예정 항목
dnt : Do Not Track. 0 = 트래킹 허용, 1 = 트래킹 차단
js : javascript 지원 여부


## 3.7.1 Object : Geo

 Name      |  Type                | 필수, 기본값 | Description                                         
:----------|:---------------------|:--------|:----------------------------------------------------
 lat       |  float; recommended  | 필수      | 위도                                                  
 lon       |  float; recommended  | 필수      | 경도                                                  
 type      |  integer; optional   | 기본값     | 위치 데이터의 출처 위도/경도를 전달할 때 권장됩니다. 목록 5.20을 참조하세요.
 ipservice |  integer             | 기본값     | IP의 경우 ISP를 나타냅니다. 3 = MaxMind                    
 country   |  string; recommended | 필수      | ISO-3166-1-alpha-3을 사용하는 국가 코드입니다.                                  
 region    |  string; recommended | 필수      | ISO-3166-2를 사용하는 지역 코드 (ex : 11)                             
 city      |  string; Optional    |         | 무역 및 운송 장소에 대한 UN 코드를 사용하는 도시입니다. (ex : Seoul)                                             
 ext       |  object              |         | 확장 필드                                               

<br>

## 3.8 Object: User

디바이스 사용자의 정보를 나타냅니다.

 Name       | Type   | 필수, 기본값 | Description                                                                                                                  
:-----------|:-------|:--------|:-----------------------------------------------------------------------------------------------------------------------------
 id         | string | 필수      | Techlabs 사용자 ID (App일 경우 ADID)                                                                                               
 buyeruid   | string | 필수      | Techlabs 사용자 ID입니다.                                                                                                          
 keywords   | string |         | 키워드, 관심사 또는 의도를 쉼표로 구분한 목록입니다.                                                                                               
 customdata | string |         | 거래소의 쿠키에 설정된 입찰자 데이터를 전달하는 선택적 기능입니다. 문자열은 base85 쿠키 안전 문자여야 하며 모든 형식이어야 합니다. "이스케이프 처리된" 따옴표를 포함하려면 적절한 JSON 인코딩을 사용해야 합니다. 

## 3.9 Object: Video

비디오 광고. 비디오 광고 요청시 반드시 포함되어야 합니다.

 Name           | Type          | 필수, 기본값      | Description                     
:---------------|:--------------|:-------------|:--------------------------------
 mimes          | string array  | 필수           | 콘텐츠 MIME 유형                     
 minduration    | Integer       |              | 최소 동영상 광고 재생 시간(초)입니다.          
 maxduration    | Integer       |              | 최대 동영상 광고 재생 시간(초)입니다.          
 protocols      | Integer array | 필수           | 3 (VAST 3.0)                    
 w              | Integer       |              | 비디오 플레이어의 너비(픽셀)입니다.            
 h              | Integer       |              | 비디오 플레이어의 높이(픽셀)입니다.            
 linearity      | Integer       | 필수           | 노출이 선형,비선형 여부를 나타냅니다.           
 skip           | Integer       | 필수           | 플레이어가 동영상 건너뛰기를 허용할지 여부를 나타냅니다. 
 skipmin        | Integer       | skip true 필수 | skip을 할 수 있는 최소 비디오 시간(초)입니다.   
 skipafter      | Integer       | skip true 필수 | skip 전에 비디오가 재생되어야 하는 시간(초)     
 battr          | Integer array |              | 차단 광고 속성 입니다.                   
 playbackmethod | Integer array | 필수           | 허용된 재생 방법입니다.                   
 pos            | Integer       |              | 화면의 광고 위치                       

<br/><br/>

# 4. 입찰 응답(Bid Response Specification)

## 4.1 Object: BidResponse

 Name    | Type         | 필수, 기본값 | Description                                  
:--------|:-------------|:--------|:---------------------------------------------
 id      | string       | 필수      | Bid Request의 ID                              
 seatbid | object array | 필수      |
 bidid   | string       |         | 응답의 ID로 입찰자가 응답을 추적하기 위해 사용함. 입찰자에 의해 선택됩니다. 
 cur     | string       | 필수      | ISO–4217 코드의 단위통화.                           

최상위 오브젝트로 id는 BidRequest의 ID를 그대로 사용합니다. 최소 하나의 seatbid 오브젝트가 필수이며, 하나의 imp에 대한 입찰을 포함합니다.

## 4.2 Object: SeatBid

 Name | Type         | 필수, 기본값 | Description    
:-----|:-------------|:--------|:---------------
 bid  | object array | 필수      | imp 대한 응답 오브젝트 
 seat | string       |         | 입찰하는 입찰 자격 코드  

## 4.3 Object: Bid

 Name    | Type         | 필수, 기본값 | Description                                        
:--------|:-------------|:--------|:---------------------------------------------------
 id      | string       | 필수      | 입찰자가 트래킹에 사용할 유니크한 아이디                             
 impid   | string       | 필수      | 응답에 대한 요청 Imp 오브젝트의 아이디                            
 price   | float        | 필수      | CPM 단위의 입찰                                         
 adid    | string       |         | 낙찰시 전송될 광고 ID                                      
 nurl    | string       |         | 낙찰시 통보 URL                                         
 lurl    | string       |         | 유찰시 통보 URL                                         
 adm     | string       |         | 광고 마크업                                             
 adomain | string array | 필수      | 광고주 최상위 도메인(광고주 필터링에 사용)                           
 bundle  | string       |         | 어플리케이션일 경우 패키지 명(앱광고등)                             
 iurl    | string       | 필수      | 광고 컨텐츠 확익을 위한 샘플 이미지 URL                           
 cid     | string       | 필수      | 광고 캠페인 ID                                          
 crid    | string       | 필수      | 광고소재 ID                                            
 cat     | string array |         | 광고소재의 컨텐츠 카테고리 목록. IAB OpenRTB Spec 2.5 > 표 5.1 참조 
 attr    | string array |         | 광고소재 속성. OpenRTB Spec 2.5 > 표 5.3 참조               
 w       | integer      |         | 광고소재 넓이(pixel)                                     
 h       | integer      |         | 광고소재 높이(pixel)                                     

## 4.4 매크로 치환 (Substitution Macros)

입찰 시 Techlabs 은 낙찰통보(Win Notice) URL 을 ${AUCTION_PRICE} 매크로를 포함하여 응답합니다.
낙찰통보 를 받을 Object 는 계약시 burl, nurl 등으로 사전에 지정되어야 하며, 낙찰시 매체에서는 매크로 값을 치환하여 호출해 주어야 합니다.

 Macro            | Description                      
:-----------------|:---------------------------------
 ${AUCTION_PRICE} | 낙찰 가격 (매체에서는 낙찰시 매크로 치환 후 호출 필요) 

<br/><br/>

# 5. Native 규격

Techlabs Native는 OpenRTB-Native-Ads-Specification-Final-1.2 를 기본으로 구성되었습니다.

## 5.1 입찰 요청

입찰 요약 규격(상세 정보는 OpenRTB-Native-Ads-Specification 1.2 참조)

## 5.1.1 Native Markup Request Object

 Name          | Type         | 필수, 기본값 | Description                         
:--------------|:-------------|:--------|:------------------------------------
 ver           | string       |         | 네이티브 요청 마크업 버전                      
 assets        | object array | 필수      | 입찰에 필요한 네이티브 구성요소를 정의한 assets 객체    
 eventtrackers | object array |         | 네이티브 이벤트 트래커 객체 - 계약시 사용할 이벤트 확정 필요 

## 5.1.2 Asset Request Object

 Name     | Type    | 필수, 기본값 | Description     
:---------|:--------|:--------|:----------------
 id       | integer | 필수      | 고유한 asset id    
 required | integer |         | 1인경우 필수값        
 title    | object  | 권장      | 타이틀 요청 asset 객체 
 image    | object  | 권장      | 이미지 요청 asset 객체 
 data     | object  |         | 데이타 요청 asset 객체 

## 5.1.2.1 Title Request Object

 Name | Type    | 필수, 기본값 | Description                              
:-----|:--------|:--------|:-----------------------------------------
 len  | integer | 필수      | 최대 타이틀 길이 - 광고 타이틀이 len 보다 긴 경우 "..." 처리 

## 5.1.2.2 Image Request Object

 Name | Type    | 필수, 기본값 | Description                             
:-----|:--------|:--------|:----------------------------------------
 type | integer | 필수      | 이미지 타입<br/>1: 로고 또는 앱 아이콘<br/>3: 메인 이미지 
 w    | integer | 권장      | 이미지 width                               
 h    | integer | 권장      | 이미지 height                              
 wmin | integer |         | 이미지 최소 width                            
 hmin | integer |         | 이미지 최소 height                           

## 5.1.2.3 Data Request Object

 Name | Type    | 필수, 기본값 | Description                                                                                                      
:-----|:--------|:--------|:-----------------------------------------------------------------------------------------------------------------
 type | integer | 필수      | 데이터 타입<br/>1: ADVERTISER<br/>2: DESCRIPTION<br/>12: CTA_DESCRPTION (버튼문구)<br/>500: OptOut 링크<br/>501: OptOut 아이콘 
 len  | integer |         | 데이터 최대 길이 - 데이터 문자열이 len 보다 긴 경우 "..." 처리                                                                        

## 5.2 입찰 응답

입찰 응답 규격(상세 정보는 OpenRTB-Native-Ads-Specification 1.2 참조)

## 5.2.1 Native Markup Response Object

 Name          | Type         | 필수, 기본값 | Description                                     
:--------------|:-------------|:--------|:------------------------------------------------
 ver           | string       |         | 네이티브 응답 마크업 버전                                  
 assets        | object array | 필수      | 입찰에 사용될 네이티브 구성요소를 정의한 assets 객체                
 link          | object array | 필수      | 광고 클릭정보를 담고있는 link 객체 (클릭 시 link>url 주소를 호출해야함) 
 eventtrackers | object array |         | 네이티브 이벤트 트래커 객체 - 계약시 사용할 이벤트 확정 필요             
 imptrackers   | string array |         | 네이티브 노출 트래커 리스트 - 계약시 사용 여부 확정 필요               

<br/><br/>

# 6. 비딩 입찰 요청 응답 예제(Bid Request Response Samples)

## 6.1 Bid Requests

### 6.1.1 Example - 디스플레이 광고 요청
(Web)
```json
{
  "id": "ac324e42-a0d6-4d8d-a665-5f00d65a39bd",
  "site": {
    "id": "ac324e42-a0d6-4d8d-a665-5f00d65a39bd",
    "name": "ADOP",
    "domain": "https://adop.cc",
    "cat": [ "IAB3-1" ],
    "privacypolicy": 1,
    "publisher": {
      "name": "adopMedia",
      "id": "a345434e-31ba-488c-939c-290c48d577e4"
    }
  },
  "device":{
    "ua":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36",
    "geo": {
      "lat": 37.5985,
      "lon": 126.9783,
      "type": 2,
      "city": "Seoul",
      "country": "KOR",
      "region": "11"
    },
    "ip":"1.222.236.235",
    "deviceType": 2,
    "os":"Mac",
    "ovs": "10",
    "ifa": "a345434e-31ba-488c-939c-290c48d577e4",
    "carrier": "SK Telecom" ,
    "ext": ""
  },
  "user": {
    "buyeruid": "techlabs-example-buyer-id"
  },
  "at": 2,
  "cur": [
    "USD"
  ],
  "test": 0,
  "imp": [
    {
      "id": "f17e285f-8b9a-4571-b7fb-0a881292238c",
      "banner": {
        "id": "8bf85894-532b-4dc7-a486-dad2fbac8e71",
        "w": 728,
        "h": 90,
        "btype": [
          3,
          4
        ],
        "battr": [
          3,
          6,
          15
        ]
      },
      "instl": 0,
      "secure": 1,
      "tagId": "b8f66a02-8475-446c-a818-4e347f281e71",
      "bidfloorcur": "USD"
    }
  ],
  "badv": [
    "block.com",
    "adv_test.com"
  ]
}
```

(App)
```json
{
   "id":"fa5745b7-4216-4cf6-a5ca-fdb275e54ee5",
   "app":{
      "id":"8f6f13b5-04a9-4341-90a3-4844b4190bf0",
      "name":"techlabs-test-app",
      "bundle":"com.techlabs.openrtb.test",
      "storeurl": "https://play.google.com/store/apps/details?id=tech.adop.testapp"
   },
   "device":{
      "ua":"Mozilla/5.0 (iPhone; CPU iPhone OS 14_0_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0 Mobile/15E148 Safari/604.1",
      "geo": {
        "lat": 37.5985,
        "lon": 126.9783,
        "type": 1,
        "city": "Seoul",
        "country": "KOR",
        "region": "11"
      },
      "ip":"1.222.236.235",
      "deviceType": 4,
      "os":"IOS",
      "ovs": "15",
      "ifa": "a345434e-31ba-488c-939c-290c48d577e4",
      "carrier": "SK Telecom" ,
      "ext": ""
   },
   "at":2,
   "cur":[
      "USD"
   ],
   "test":0,
   "tmax":1000,
  "imp":[
    {
      "id":"d2c36366-d603-4c27-9ccf-0169fd0caaf3",
      "banner":{
        "w":640,
        "h":480
      },
      "instl":0,
      "secure":1,
      "bidfloorcur":"USD"
    }
  ],
  "badv": [
    "block.com",
    "adv_test.com"
  ]
}
```

### 6.1.2 Example - Native 광고 요청
(Web)
```json
{
  "id": "292a5b28-b96f-478a-a575-662b5110d746",
  "tmax": 500,
  "site": {
    "id": "f592e936-c774-4cf2-81c7-1d70226454ad",
    "name": "ADOP",
    "domain": "https://adop.cc",
    "privacypolicy": 1,
    "cat": [
      "IAB3-1"
    ],
    "publisher": {
      "name": "adopMedia",
      "id": "a345434e-31ba-488c-939c-290c48d577e4"
    }
  },
  "user": {
    "buyeruid": "techlabs-example-buyer-id"
  },
  "at": 2,
  "cur": [
    "KRW"
  ],
  "test": 0,
  "imp": [
    {
      "id": "c204a055-3ed8-431d-8d63-abf39f64836a",
      "instl": 0,
      "secure": 1,
      "native": {
        "ver": "1.1",
        "request": "{\"native\":{\"ver\":\"1.1\",\"plcmttype\":1,\"plcmtcnt\":1,\"seq\":0,\"assets\":[{\"id\":1,\"required\":0,\"data\":{\"type\":12,\"len\":1000}},{\"id\":2,\"required\":1,\"title\":{\"len\":100}},{\"id\":3,\"required\":1,\"img\":{\"type\":1,\"w\":80,\"wmin\":0,\"h\":80,\"hmin\":0}},{\"id\":4,\"required\":1,\"img\":{\"type\":3,\"w\":1200,\"wmin\":0,\"h\":627,\"hmin\":0}},{\"id\":5,\"required\":0,\"data\":{\"type\":3,\"len\":1000}},{\"id\":6,\"required\":1,\"data\":{\"type\":2,\"len\":150}}]}}"
      },
      "tagId": "b8f66a02-8475-446c-a818-4e347f281e71",
      "bidfloorcur": "USD"
    }
  ]
}
```

(App)
```json
{
  "id": "292a5b28-b96f-478a-a575-662b5110d746",
  "tmax": 500,
  "app":{
    "id":"8f6f13b5-04a9-4341-90a3-4844b4190bf0",
    "name":"techlabs-test-app",
    "bundle":"com.techlabs.openrtb.test",
    "storeurl": "https://play.google.com/store/apps/details?id=tech.adop.testapp"
  },
  "device":{
    "ua":"Mozilla/5.0 (iPhone; CPU iPhone OS 14_0_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0 Mobile/15E148 Safari/604.1",
    "geo": {
      "lat": 37.5985,
      "lon": 126.9783,
      "type": 2,
      "city": "Seoul",
      "country": "KOR",
      "region": "11"
    },
    "ip":"1.222.236.235",
    "devicetype": 4,
    "os":"IOS",
    "ovs": "15",
    "ifa": "a345434e-31ba-488c-939c-290c48d577e4",
    "carrier": "SK Telecom" ,
    "ext": ""
  },
  "user": {
    "buyeruid": "techlabs-example-buyer-id"
  },
  "at": 2,
  "cur": [
    "KRW"
  ],
  "test": 0,
  "imp": [
    {
      "id": "c204a055-3ed8-431d-8d63-abf39f64836a",
      "instl": 0,
      "secure": 1,
      "native": {
        "ver": "1.1",
        "request": "{\"native\":{\"ver\":\"1.1\",\"plcmttype\":1,\"plcmtcnt\":1,\"seq\":0,\"assets\":[{\"id\":1,\"required\":0,\"data\":{\"type\":12,\"len\":1000}},{\"id\":2,\"required\":1,\"title\":{\"len\":100}},{\"id\":3,\"required\":1,\"img\":{\"type\":1,\"w\":80,\"wmin\":0,\"h\":80,\"hmin\":0}},{\"id\":4,\"required\":1,\"img\":{\"type\":3,\"w\":1200,\"wmin\":0,\"h\":627,\"hmin\":0}},{\"id\":5,\"required\":0,\"data\":{\"type\":3,\"len\":1000}},{\"id\":6,\"required\":1,\"data\":{\"type\":2,\"len\":150}}]}}"
      },
      "tagId": "b8f66a02-8475-446c-a818-4e347f281e71",
      "bidfloorcur": "USD"
    }
  ]
}
```

### 6.1.3 Example - Video 광고 요청
(Web)
```json
{
  "id": "bf662c35-41b8-429a-b3d0-904ca258bda4",
  "site": {
    "id": "f4e12022-0d8e-40da-a3ae-5f49253733e4",
    "name": "ADOP",
    "domain": "https://adop.cc",
    "privacypolicy": 1,
    "cat": [
      "IAB3-1"
    ],
    "publisher": {
      "name": "adopMedia",
      "id": "a345434e-31ba-488c-939c-290c48d577e4"
    }
  },
  "user": {
    "buyeruid": "techlabs-example-buyer-id"
  },
  "at": 2,
  "cur": [
    "KRW"
  ],
  "test": 0,
  "imp": [
    {
      "id": "e6d63b12-286d-4f5d-a2dd-55b09b254a84",
      "video": {
        "mimes": [
          "video/mp4"
        ],
        "protocols": [
          3
        ],
        "linearity": 1,
        "skip": 0,
        "skipmin": 5,
        "skipafter": 5,
        "playbackmethod": [
          2,
          3,
          6
        ],
        "w": 640,
        "h": 480
      },
      "instl": 0,
      "secure": 1,
      "tagId": "b8f66a02-8475-446c-a818-4e347f281e71",
      "bidfloorcur": "USD"
    }
  ]
}
```

(App)
```json
{
   "id":"5725a6da-364f-4685-830c-8a156547e5c3",
   "app":{
      "id":"8f6f13b5-04a9-4341-90a3-4844b4190bf0",
      "name":"techlabs-test-app",
      "bundle":"com.techlabs.openrtb.test",
      "storeurl": "https://play.google.com/store/apps/details?id=tech.adop.testapp"
   },
   "device":{
      "ua":"Mozilla/5.0 (iPhone; CPU iPhone OS 14_0_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0 Mobile/15E148 Safari/604.1",
      "geo": {
        "lat": 37.5985,
        "lon": 126.9783,
        "type": 1,
        "city": "Seoul",
        "country": "KOR",
        "region": "11"
      },
      "ip":"1.222.236.235",
      "devicetype": 4,
      "os":"IOS",
      "ovs": "15",
      "ifa": "a345434e-31ba-488c-939c-290c48d577e4",
      "carrier": "SK Telecom" ,
      "ext": ""
   },
   "at":2,
   "cur":[
      "USD"
   ],
   "test":0,
   "tmax":1000,
   "imp":[
      {
         "id":"d55b70a3-7ae1-4111-be99-af00dc1fc413",
         "video":{
            "mimes":[
               "video/mp4"
            ],
            "minduration":5,
            "maxduration":60,
            "w":640,
            "h":480
         },
         "instl":0,
         "secure":1,
         "bidfloorcur":"USD",
         "ext": {
           "allctvs": 1,
           "rewarded": 1
         }
      }
   ]
}
```

<br/><br/>

# 7. 쿠키교환 Cookie Matching - Cookie Sync

쿠키교환과 입찰요청은 아래와 같은 flow 로 진행 됩니다.

 순서 | 내용                                                                                      | 담당       
:---|:----------------------------------------------------------------------------------------|:---------
 1  | Techlabs는 쿠키교환을 위한 end-point 를 DSP에 으로 제공합니다 (https 필수)                                 | Techlabs 
 2  | 사용자가 광고주 사이트에 나타난 경우, 제공받은 end-point 로 "DSP 사용자 ID" 추가하여 GET 방식으로 요청합니다.                | DSP      
 3  | Techlabs는 전달받은 "DSP 사용자 ID" 와 "Techlabs 사용자 ID" 를 Techlabs Database 에 저장합니다.            | Techlabs 
 4  | Techlabs 사용자가 네트워크에 나타난 경우, Techlabs측 Database 에 저장된 "DSP 사용자 ID" 를 포함하여 DSP로 입찰 요청합니다. | Techlabs 

## 7.1 사용자 ID 교환 요청 방식

Techlabs는 DSP의 사용자 ID 값을 전달 받기위한 end-point 를 제공합니다.
HTTPS 프로토콜을 사용하며, GET 방식으로 요청합니다.
쿼리스트링에 필수값과 DSP별 옵션값을 추가해 end-point로 요청합니다.

Techlabs 제공 end-point

```
https://cookie-sync-sample.adop.cc/cookie-sync?dsp_id=[DSP_ID]
```

DSP가 호출 호출시 완성된 url 예시

```
https://cookie-sync-sample.adop.cc/cookie-sync?dsp_id=temp123&dsp_user_id=[DSP 사용자 ID]
```

```
sudo /opt/calyptia-fluentd/usr/sbin/calyptia-fluentd -c /Users/wonwoo/git/techlabs-rtb/fluendt-rtb.conf
```

