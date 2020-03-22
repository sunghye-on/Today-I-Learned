# 구글에서 제공해준 PWA 기본 예제

## 		:white_check_mark:목차

### 👉 : Introduction

### 👉 : 환경 설정

### 👉 : 기준 설정

### 👉 : 웹 앱 매니페스트 만들기

### 👉 : 기본적인 오프라인 기능 만들기

### 👉 : 전체 오프라인 기능 만들기

### 👉 : 설치가능한 웹 앱으로 만들기

### 👉 :  기본 PWA 완료!!!



### 🙏 [깃허브 주소 바로가기](https://github.com/sunghye-on/Weather-PWA-google-) 🙏

## 👉Introduction

날씨를 알려주는 웹을 기본으로 우리는 점차 진화하는 PWA로 진화할것!

### 우리가 만들 것들

- 반응형 디자인으로 데스크탑과 모비일기기에서 사용이 가능하다
- 서비스워커를 사용하여 실행에 필요한 앱의 리소스를 미리캐시하고 런타임에 날씨 데이터를 캐시하여 성능을 향샹 시킨다
- 웹 앱 매니페스트를 이용하여 설치가능하게 하고 유저에게 설치 가능하다는 노티피케이션 이벤트를 주겠다.

### 우리가 배울 것들

- 웹 앱 미니페스트를 작성하는 방법을 배운다.
- 오프라인에서 일정부분 사용가능하게 만드는 방법을 배운다.
- 오프라인에도 완벽히 작동하는 앱을 만드는 방법을 배운다.

---

## 👉SETUP

### 전체 환경 세팅

아래의 링크에서 파일을 다운 받거나 [https://glitch.com](https://glitch.com/). 를 이용한다. 나는 파일을 다운받아 VS code에서 진행하겠다. (VS code로 진행시 node.js 필수!)

#### 파일 다운로드 링크

[바로 다운로드하기](https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip)

### VS code에서 시작하기

1. 파일 압축을 풀어준 뒤 VS code 애서 폴더를 열어준다.
2. .env의 DARKSKY_API_KEY를 https://api.darksky.net/forecast/DARKSKY_API_KEY/40.7720232,-73.9732319로 수정해준다.
   - 이때 DARKSKY_API_KEY는 날씨를 받아오는 API
3. 패키지 파일들의 설치를 위해 npm i 커맨드 입력
4. 시작을 위한 스크립트 npm start 입력
5. 크롬 브라우져로 [http://localhost:8000](http://localhost:8000/) 열기

## 👉기준 설정

### Lighthouse(등대)

**Lighthouse**는 사이트 및 페이지의 품질을 향상 시키기는데 도움이되는 도구이 사용하기 쉽다!! 또한 성능이나 접근성, 그리고 PWA에 대한 감사를 실사하여 문제해결방법을 설명하거나 부족한 부분을 알려준다.

#### 실행 방법

크롬 개발자도구(F12를 누르면 나옴)에서 Aduits에서 run Audits를 클릭하면 실행된다.

#### 우리가 해결해나갈 오류들

1. Current page does not respond with a 200 when offline.
2. `start_url` does not respond with a 200 when offline.
3. Does not register a service worker that controls page and `start_url.`
4. Web app manifest does not meet the installability requirements.
5. Is not configured for a custom splash screen.
6. Does not set an address-bar theme color.

---

## 👉웹 앱 미니페스트 만들기

### 웹 앱 매니페스트란?

간단한 JSON파일로 개발자가 앱의 사용자에게 표시되는 방식을 제어 할 수 있는 기능을 제공한다!

### 웹 앱 매니페스트 생성

우리의 프로젝트 public 폴더 안에 manifest.json을 생성합니다.

그후 아래의 코드를 (입력 || 복붙) 합시다 **(!!복붙할시에는 주석 제거)**

```json
{
  // name과 이름이 너무 길 경우 표시되는short_name
  "name": "Weather",
  "short_name": "Weather",
  // 설치 및 화면에 뿌려줄 아이콘들
  // tip: 설치가능한 PWA를 위해서는 적어도 192 X 192px 아이콘과 512 X 512px 아이콘을 포함해야한다.
  "icons": [
    {
      "src": "/images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    },
    {
      "src": "/images/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  //앱을 열면 시작될 시작 URL을 정의
  "start_url": "/index.html",
  //display의 모드를 설정할 수 있다.(3가지의 모드가 있다)
  //tip: 설치가능한 PWA를 위해서는 standalone혹은 fullscreen으로 설정해야한다.
  "display": "standalone",
  "background_color": "#3E4EB8",
  "theme_color": "#2F3BA2"
}
```

### 웹 앱 매니페스트 연결 및 확인

아래의 설명에 맞는 링크들를 index.html에 head에 연결시켜준다.

매니페스트 연결을 위해 아래의 링크를 연결해준다.

```html
<link rel="manifest" href="/manifest.json" />
```

이제 매니페스트 연결을 마쳤다면

개발자도구 => Application => Manifest에서 우리가 만든 매니페스트가 적용된 것을 알 수 있다.

#### iOS를 위한 meta tag

iOS의 사파리는 아직 웹 앱 매니페스트를 지원하지 않습니다... (슬픔) 그래서 우리는 아래와 같이 고전적인? 메타태그를 직접 달아둬야합니다

```html
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<meta name="apple-mobile-web-app-title" content="Weather PWA" />
<link rel="apple-touch-icon" href="/images/icons/icon-152x152.png" />
```

#### 앱의 설명 작성

SEO의 Audits결과중에 Document does not have a meta description.가 있다. 이것은 설명이 없다는 이야기인데 설명은 구글 검색결과에 표시될 수 있습니다. 즉, 설명을 자세하고 잘 쓰면 사용자들이 검색하여 많은 사람들이 사용할 수 있습니다.

아래와 같이 혹은 원하는 설명을 적어보겠습니다.

```html
<meta name="description" content="A sample weather app" />
```

#### 주소창 색상 지정

PWA Audits 결과중 Does not set an address-bar theme color가 있는데 우리가 만드는 브랜드의 색상에 맞게 변경할 수 있는 기능입니다. 필수는 아니지만 권장합니다.

```html
<meta name="theme-color" content="#2F3BA2" />
```

다시 Run Audits를 실행하보면 우리는 기존 6개의 오류중 4,5,6번 오류를 해결했고 1,2,3번의 오류가 남은 것을 볼 수 있다.

---

## 👉기본적인 오프라인 기능 구현

이번에는 날씨앱에 대한 간단한 오프라인 페이지를 축해보도록 하겠습니다. 사용자가 오프라인 상태에서 앱을 로드하려고 하면 브라우저에 표시되는 일반적인 오프라인 페이지(공룡게임)말고 우리가 지정한 페이지가 표시되게 할 것이다.

이번 학습을 하면 우리가 위에서 봤던 1,2,3번의 오류를 해결 할 수 있습니다.

다음에는 완전한 오프라인 환경으로 바꿀것이다.

서비스 워커를 통해 제공되는 기능은 점진적 향상으로 간주되어야하며 브라우저가 지원하는 경우에만 추가해야합니다. 예를 들어 서비스 워커를 사용하면 네트워크가없는 경우에도 사용할 수 있도록 앱의 앱 셸과 앱의 데이터를 캐시 할 수 있습니다. 서비스 워커가 지원되지 않으면 오프라인 코드가 호출되지 않고 사용자에게 기본 경험(공룡게임)이 제공됩니다. 점진적 개선 기능을 제공하기 위해 기능 감지를 사용하면 오버 헤드가 거의 없으며 해당 기능을 지원하지 않는 이전 브라우저에서는 중단되지 않습니다.

```javascript
//tip
/*
*	서비스워커의 기능들은 HTTPS를 통해 액세스되는 페이지에서만 사용될 수 있습니다.
*	(http://localhost and equivalents also work to facilitate testing.)
```

### 서비스 워커 등록

서비스워커의 등록을위해 index.html의 script태그 부분에 아래 자바스크립트 코드를 삽입하여 서비스 워커를 등록해보자

```javascript
if ("serviceWorker" in navigator) {
  window.addEventListener("load", () => {
    navigator.serviceWorker.register("/service-worker.js").then(reg => {
      console.log("Service worker registered.", reg);
    });
  });
}
```

위의 코드는 서비스워커를 지원하면 실행되는 블럭이다. 그리고 로드이벤트가 발생시 우리의 서비스워커가 존재하는 위치를 지정해줌으로 서비스워커를 등록시킨다. 그후 콘솔로 성공사실을 알린다.

```javascript
//tip
/*	서비스워커는 자바스크립트들이 모여있는 script디렉토리에 존재하는 것이 아닌 루트 디렉토리에 존재한다. 
    이게 서비스 워커의 범위(scope)를 설정하는 가장 쉬운 방법이다. 루트디렉토리에 생성하면 서비스
	  워커는 모든 웹페이지에 요청을 제어 할 수 있다. 
*/
```

### Precache 오프라인 페이지

먼저 우리가 서비스워커에게 무엇을 미리 캐시할지 말해야합니다!! 이번 프로젝트에 미리 만들어진 오프라인 페이지(offilne.html)를 인터넷 연결이 없을때 보여주도록 합시다.

가장 먼저 서비스워커에 FILES_TO_CACHE에 오프라인 페이지를 넣어줍시다.

```javascript
const FILES_TO_CACHE = ["/offline.html"];
```

다음으로 우리는 서비스워커가 설치되는 install이벤트에서 우리가 지정한 FILES_TO_CACHE파일들을 캐시에 미리 저장합니다.

```javascript
self.addEventListener("install", evt => {
  console.log("[ServiceWorker] Install");
  // CODELAB: Precache static resources here.
  evt.waitUntil(
    caches.open(CACHE_NAME).then(cache => {
      console.log("[ServiceWorker] Pre-caching offline page");
      return cache.addAll(FILES_TO_CACHE);
    })
  );
  self.skipWaiting();
});
```

인스톨이벤트가 발생하면 제공된 캐시이름으로 cache.open()으로 캐시를 열수있다. 이때 캐시이름은 캐시의 버전을 관리하기 쉽게 만들어진다. 또한 캐시의 버전 관리를 하면 업데이트가 간편하다.

케시를 연이후에는 URL목록을 서버에서 가져와서 캐시에 response를 추가하는 cache.addAll()을 이용할 수 있다. URL을 요청하는 중에 하나라도 실패하면 전체가 실패한다.

### 개발자도구로 서비스워커 확인

크롬 개발자도구를 이용하여 서비스워커를 이해하고 디버깅하는 방법을 보겠습니다. 개발자도구에 Application에 Service Workers에 들어가보면 아무것도 없거나 서비스워커가 설치된 것을 볼수 있다. 아무것도 없다면 페이지를 새로고침해보자!

서비스워커에 관한 내용들이 있으면 현재 서비스워커가 작동중이라는 것이다.

### 이전 캐시를 지우기

우리는 activate이벤트를 사용하여 우리의 이전 캐시데이터를 지울 것이다. 이작업에서 서비스워커는 CACHE_NAME 의 번화를 확인하여 변화가 있을시(업데이트가 있을시) 이번의 데이터를 삭제한다. 아래의 코드에서 확인하자

```javascript
self.addEventListener("activate", evt => {
  console.log("[ServiceWorker] Activate");
  // CODELAB: Remove previous cached data from disk.
  evt.waitUntil(
    caches.keys().then(keyList => {
      return Promise.all(
        keyList.map(key => {
          if (key !== CACHE_NAME) {
            console.log("[ServiceWorker] Removing old cache", key);
            return caches.delete(key);
          }
        })
      );
    })
  );
  self.clients.claim();
});
```

### 인터넷 연결을 실패할때 캐시를 다루는 방법

마지막으로 우리는 fetch이벤트를 처리해야한다. 우리는 "Network falling back to cache" 방법을 사용할 것이며 서비스워커가 먼저 인터넷에서 fetch 리소르를 받아오고자 한다. 만약 실패하면 서비스워커가 캐시에 있던 오프라인 페이지를 가져오도록하는 방법이다.

아래와 같은 코드를 fetch이벤트 핸들러에 작성해보자

```javascript
self.addEventListener("fetch", evt => {
  console.log("[ServiceWorker] Fetch", evt.request.url);
  // CODELAB: Add fetch event handler here.
  if (evt.request.mode !== "navigate") {
    // Not a page navigation, bail.
    return;
  }
  evt.respondWith(
    fetch(evt.request).catch(() => {
      return caches.open(CACHE_NAME).then(cache => {
        return cache.match("offline.html");
      });
    })
  );
});
```



위의 코드를 해석해보면 fetch이벤트에서는 페이지의 request모드가 navigate가 아니라면 다른 핸들러에 요청을 보내고 바로 fetch함수를 이용하여 이벤트를 요청한뒤 실패하면(.catch)가장 최근의 캐시이름의 캐시저장소에서 캐시를 연뒤 offilne.html과 일치하는 파일을 찾아 반환한다.

이때 evt.respondWith()로 fetch 함수를 감싸야지 브라우저의 기본페치처리가 방지되고 브아루저가 응답을 직접 처리하도록 지시한다. 즉 evt.respondWith()를 호출하지 않으면 기본 네트워크 동작만 얻게된다.



### 개발자 도구및 결과 확인해보기

이전에 보았던 Application의 SErvice Worker에 들어간뒤 새로고침을 하면 서비스워커의 숫자가 오르면서 새로운 버전의 서비스워커가 설치된 것을 볼 수 있다. 또한 우리는 아랫부분에 Cache Storage에서 refresh버튼을 누르면 offline.html이라는 캐시가 생긴 것을 확인 할 수 있다. 

자, 이제 개발자도구의 Network탭에 들어간뒤 offline을 선택하여 오프라인환경을 만든뒤에 새로고침을 해보자 그러면 크롬 공룡게임이 아닌 우리가 미리 만들어놓은 오프라인 페이지가 나타날것이다!!



이제 우리는 기존에 있던 1,2,3,4,5,6번의 오류를 모두 해결했습니다.!!!1 다음에는 전체 오프라인 서비스를 제공해봅시다!!





## 👉전체 오프라인 기능 만들기

### 서비스 워커의 생명 주기(life cycle)

서비스워커의 생명 주기는 가장 복잡한 부분입니다! 하지만 작동방식을 알고나면 해당 웹의 서비스마다 적절한 패턴을 혼합시켜서 매끄러운 서비스가 가능합니다. 여기서는 서비스워커에 관한 내용들을 조금 스킵할 생각이며 자세한 사항을 공부하기 위해서 아래의 링크에 접속해 봅시다.



[구글](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle)

[제가 만든 github PWA저장소 4장 참고](https://github.com/sunghye-on/Today-I-Learned/tree/master/PWA)

### app의 로직을 업데이트

앱은 캐시와 네트워크에 각각 하난씩 두개의 비동기 요청을 해야한다. 앱은 창에서 사용 가능한 캐시 개체를 사용하여 캐시에 액세스하고 최신 데이터를 검색한다. 이것은 캐시 객체가 모든 브라우저에서 사용 가능하지 않을 수 있으므로 점진적 향상의 훌륭한 예이다. 만약 브라우져에서 캐시를 사용할 수 없을 경우 계속 네트워크 요청을 해야합니다.

getForecastFromCache() 함수를 업데이트 하여 글로벌 window객체에서 캐시 객체를 사용할 수 있는지 확인 하고 캐시가 있으면 캐시에서 데이터를 요청하는 로직을 만들어보자! 

```javascript
//public/script/app.js의 함수를 수정

function getForecastFromCache(coords) {
  // CODELAB: Add code to get weather forecast from the caches object.
  if (!("caches" in window)) {
    return null;
  }
  const url = `${window.location.origin}/forecast/${coords}`;
  return caches
    .match(url)
    .then(response => {
      if (response) {
        return response.json();
      }
      return null;
    })
    .catch(err => {
      console.error("Error getting data from cache", err);
      return null;
    });
}
```



그런다음 updateData() 함수를 수정하여 네트워크에서 가져오기 위해 getForecastFromNetwork()를 호출하고 최신 캐시된 것을 가져오기 위해 getForecastFromCahce()를 호출합니다!



```javascript
function updateData() {
  Object.keys(weatherApp.selectedLocations).forEach(key => {
    const location = weatherApp.selectedLocations[key];
    const card = getForecastCard(location);
    // CODELAB: Add code to call getForecastFromCache.
    getForecastFromCache(location.geo).then(forecast => {
      renderForecast(card, forecast);
    });
    // Get the forecast data from the network.
    getForecastFromNetwork(location.geo).then(forecast => {
      renderForecast(card, forecast);
    });
  });
}
```

이제 우리의 날씨앱은 캐시와 네트워크 요청으로 두개의 비동기 요청을 수행합니다. 캐시에 데이터가 있으면 반환되어 아주 빠르게 렌더링 되고 그다음에 네트워크에서 응답이 오면 최신 데이터가 업데이트 됩니다!

우리는 어떻게 캐시요청과 fetch요청이 동시에 끝난것을 알 수 있을까요?  어떻게 앱에서 최신데이터를 보여주고 있다고 우리는 알 수 있을까요?   그것은 바로 아래의 renderForecast()함수중 일부에서 알 수 있습니다, 아래의 코드를 확인합니다.

```javascript
if (lastUpdated >= data.currently.time) {
    return;
  }
```

바로 이 부분입니다. 매번 날씨카드가 업데이트 될 때마다 앱은 숨겨진 곳에 시간 기록(타임 스템프)을 저장해놓는다. 카드에 이미 존재하는 시간 기록이 함수에 전달된 데이터 보다 최신인 경우 앱을 중단합니다!!



### 우리의 resources들을 캐시에 저장하기

 캐시들을 미리 저장하기전에 우리는 CACHE_NAME 의 버전을 한단계올려 static-cache-v2로 만들어준뒤 DATA_CACHE_NAME 을 data-cache-v1로 추가해 줍시다!  

DATA_CACHE_NAME 는 앱의 데이터와 앱 쉡과 분리해 줄 수 있는 기능이다. 예를 들어 앱에서 바뀌지 않는 부분을 앱 쉘로 분리하면 앱캐시에서 새로운 정보를 받아와 업데이트하는동안 리미 준비되어있는 앱 쉘이 아주아주 빠르게 렌더링해준다. 

아래와같이 서비스워커의 코드를 수정하자.

```javascript
const CACHE_NAME = "static-cache-v2";
const DATA_CACHE_NAME = "data-cache-v1";
```

또한 우리의 앱이 오프라인으로 구동이 가능할려면 우리의 resource들이 미리 캐싱되어야하는데 이것은 앱이 요청을 네트워크에서 하는 것이 아닌 로컬 캐시에서 모든 리소스를 로드할 수 있기때문에 불안정한 네트워크에서도 가능하다.

 FILES_TO_CACHE의 파일들을 아래와 같이 수정하자

```javascript
const FILES_TO_CACHE = [
  "/",
  "/index.html",
  "/scripts/app.js",
  "/scripts/install.js",
  "/scripts/luxon-1.11.4.js",
  "/styles/inline.css",
  "/images/add.svg",
  "/images/clear-day.svg",
  "/images/clear-night.svg",
  "/images/cloudy.svg",
  "/images/fog.svg",
  "/images/hail.svg",
  "/images/install.svg",
  "/images/partly-cloudy-day.svg",
  "/images/partly-cloudy-night.svg",
  "/images/rain.svg",
  "/images/refresh.svg",
  "/images/sleet.svg",
  "/images/snow.svg",
  "/images/thunderstorm.svg",
  "/images/tornado.svg",
  "/images/wind.svg"
];
```



파일들을 하나하나 보면 앱의구동에 필요한 파일들을 모두 가져온 것을 알 수 있고 offline.html이 없어진 점도 볼 수 있다.  즉 우리는 오프라인에서 공룡게임 페이지도 아닌 offline.html도 아닌 기존에 서비스를 그대로 사용할 수 있다.



### activate 이벤트 및 fetch 이벤트 핸들러 업데이트

activate 이벤트에서 이벤트 핸들러가 실수로 data를 삭제하지 않게 하기 위해서  if (key !== CACHE_NAME)을 아래와 같이 바꿔준다.  CACHE_NAME와 DATA_CACHE_NAME 둘다 같지 않을시에만 삭제 해줌!

```javascript
if (key !== CACHE_NAME && key !== DATA_CACHE_NAME)
```



우리는 날씨 API로 부터 받아온 요청들을 서비스워커가 가로채서 캐시저장소에 저장하여 다음에 쉽게 접근 할 수 있도록 서비스워커의 fetch이벤트 핸들러를 수정해줘야합니다!!  우리의  stale-while-revalidate strategy 에서 우리는 네트워크에 접근 하여 가장 사실에 입각한 가장 최신의 정보를 얻고자 하며 실패시에 미리 캐시해놓은 가장 최근의 데이터를 보여줍니다!

fetch이벤트 핸들러를 아래와 같이 수정해봅시다!

```javascript
self.addEventListener("fetch", evt => {
  console.log("[ServiceWorker] Fetch", evt.request.url);
  // CODELAB: Add fetch event handler here.
  if (evt.request.url.includes("/forecast/")) {
    console.log("[Service Worker] Fetch (data)", evt.request.url);
    evt.respondWith(
      caches.open(DATA_CACHE_NAME).then(cache => {
        return fetch(evt.request)
          .then(response => {
            // If the response was good, clone it and store it in the cache.
            if (response.status === 200) {
              cache.put(evt.request.url, response.clone());
            }
            return response;
          })
          .catch(err => {
            // Network request failed, try to get it from the cache.
            return cache.match(evt.request);
          });
      })
    );
    return;
  }
  evt.respondWith(
    caches.open(CACHE_NAME).then(cache => {
      return cache.match(evt.request).then(response => {
        return response || fetch(evt.request);
      });
    })
  );
});
```

 이 코드는 만약에 일기예보와 관련된 요청이라면 그것을 가로챈뒤 fetch request를 만든다. 응답이 리턴되면 캐시를 열고 응답을 복제해서 캐시에 저장한뒤에 원래 요청했던 곳으로 돌아간다.

우리는  evt.request.mode !== 'navigate ' 해당 코드를 지워하 한다. 왜냐면 우리는 서비스워커가 단순히 navigate가 아닌 모든 request를 처리해 주길 바란다.  만약에 그대로 두면 우리는 HTML파일만 제공될 것이고 나머지는 모두 네트워크에서 요청된다.

### 직접 해봅시다!!

이제 직접 우리가 만든 오프라인 서비스들을 확인해봅시다. 

최신버전의 서비스워커를 설치를 위해 새로고침을 한번 해줍시다! *(혹시 서비스워커가 업데이트가 안됐다면 페이지를 껐다가 킨후 새로고침해주세요)*  그후 새로운 도시들을 추가한뒤 개발자 도구 Application에 Cache Storage를 들어가보면 현재 서비스워커의 static캐시버전과 data캐시버전이 있고 우리가 저장했던 것을 을 확인할 수 있다.

이제 오프라인 모드로 설정후에 새로고침을 해보면 오프라인 이지만 날씨 정보를 확인 할 수 있다!!! 이때 큰 체감을 못느낀다면 개발자도구 Network에서 slow 3G로 설정하고 다른 홈페이지를 들어가 본 뒤 우리의 미리캐시된 페이지를 들어가보면 아주아주 빠르다는 것을 알 수 있다!!

--------------------

## 👉설치가능한 웹 앱으로 만들기

PWA(Progressive Web App)가 설치되면 설치된 다른 앱처럼 보이고 앱처럼 작동한다. 크롬에서는 PWA를 점3개가 있는 메뉴버튼으로 설치할 수 있고 또한 사용자에게 앱을 설치하라는 UI를 제공할 수 있다. 하지만 크롬에서 메뉴에서 직접 설치하는것은 일반 사용자는 알기 쉽지 않기때문에 설치를 위한 프로세스를 만드는것이 바람직합니다!!!



### 개발자도구 Audits의 Lighthouse를 이용한 설치가능 확인 

위에서 설명한 등대기능으로 설치가능 여부를 확인 할 수 있다. 우리의 페이지를 보면 3가지의 조건이 모두 만족하는 것을 볼 수 있다. 

### install.js 추가하기

먼저 install.js를 index.html에 추가합니다. 방법은 일반 js 파일을 추가하는 것과 같습니다. 아래의 코드 참고

```html
<script src="/scripts/install.js"></script>
```

### beforeinstallprompt 이벤트 

만약 홈화면에 추가할 수 있는 조건들이 만족되면 크롬에서 설치하라는 메시지를 보내기 전에 beforeinstallprompt 이벤트를 발생시킨다.  beforeinstallprompt 이벤트의 추가를 위해 아래의 코드를 public/script/install.js에 추가해봅시다.

```javascript
window.addEventListener('beforeinstallprompt', saveBeforeInstallPromptEvent);
```

### Save event와 install 버튼 보이기

saveBeforeInstallPromptEvent 함수에서 beforeinstallprompt 이벤트에 대한 참조를 저장하여 나중에 prompt ()를 호출하고 UI를 업데이트하여 설치 버튼을 표시 할 수 있습니다. 아래의 코드를 추가해보자

```javascript
function saveBeforeInstallPromptEvent(evt) {
  // CODELAB: Add code to save event & show the install button.
  deferredInstallPrompt = evt;
  installButton.removeAttribute("hidden");
}
```

### prompt를 보이고 버튼을 숨기자

사용자가 설치버튼을 누르면 우리는 beforeinstallprompt에 저장된 .prompt()를 호출해야한다. 또한 우리는 .prompt버튼은 한번만 사용할 것이므로 설치버튼을 숨겨야한다. 아래의 코드를 installPWA 함수에 추가하자

```javascript
function installPWA(evt) {
  // CODELAB: Add code show install prompt & hide the install button.
  deferredInstallPrompt.prompt();
  evt.srcElement.setAttribute("hidden", true);
}
```

 .prompt() 를 호출하면 사용자에게 모달의 대화상자가 표시되며 앱을 홈화면에 추가하도록 요청합니다.



### 사용자의 설치여부 기록

우리는 저장된 beforeinstallprompt이벤트의 프로미스를 통해 반환된 **userChoice** 속성으로 사용자가 설치하라는 모달창에서 어떤 대답을 했을 지 알 수 있다. 이 프로미스는 outcome이라는 속성을 반환하는데 이것은 사용자가 어떤 반응을 했을지 알 수 있는 속성이다. 아래의 코드를 이용하여 installPWA함수를 수정하자

```javascript
function installPWA(evt) {
  // CODELAB: Add code show install prompt & hide the install button.
  deferredInstallPrompt.prompt();
  // Hide the install button, it can't be called twice.
  evt.srcElement.setAttribute("hidden", true);
  // CODELAB: Log user response to prompt.
  deferredInstallPrompt.userChoice.then(choice => {
    if (choice.outcome === "accepted") {
      console.log("User accepted the A2HS prompt", choice);
    } else {
      console.log("User dismissed the A2HS prompt", choice);
    }
    deferredInstallPrompt = null;
  });
}
```

이때 userChoice는 함수가 아닌 [여기](https://w3c.github.io/manifest/#beforeinstallpromptevent-interface) 이곳에 정의 되어 있는 속성이다.

### 모든 설치이벤트 기록하기

만약에 사용자가 점3개 메뉴에서 직접 설치하는것 처럼 다른 방법으로 설치할 때도 해당 이벤트를 추적해서 기록할 수 있다.  아래의 코드를 추가하자

```javascript
window.addEventListener('appinstalled', logAppInstalled);
```

자 이제 logAppInstalled 함수를 수정해보자 여기서는 단순히 console.log만을 적겠지만 실제 제공되는 앱에서는 아마 제공하는 서비스마다 분석을 위한 다른 기록을 원할 것이다. 우리는 아래와 같이 console.log를 찍어보자

```javascript
function logAppInstalled(evt) {
  // CODELAB: Add code to log the event
  console.log("Weather App was installed.", evt);
}
```



### 서비스워커 업데이트

우리는 이미 캐시된 파일을 변경했으니까 CACHE_NAME을 엡데이트하여 버전업된 캐시파일을 엡데이트 해야합니다. 개방자 도구에서  **Bypass for network**를 선택할 수 있지만 이것은 개발 용이지 실제 앱에는 영향을 주지 않는다.



### 자, 이제 해봅시다

설치단계가 어떻게 진행되는지 봅시다! 우선 안전을 위해 Application에세 데이터를 한번 지우고 새로 시작합시다.

**먼저 상단 네브바에서 설치아이콘이 잘 표시되는지 확인해봅시다.**

1. 우리의URL(여기서는 localhost)을 열어줍니다. 
2. 크롬의 주소창 옆에 점3개 메뉴를 열어줍니다.
   * Weather설치.. 를 찾을 수 있을겁니다!
3. 새로고침을 하여 날씨데이터를 새로고침해주고 우측상단의 설치가능한 아이콘이 생긴 것을 확인 할 수 있을 것입니다!!

**다음으로 install 버튼이 정상적으로 작동하는지 확인해봅시다.**

우리는 모든 설치가 올바르게 작동하고 있는 지 테스트 가능하고 데스크탑이나 모바일로 이 작업을 수행할 수 있다. 모바일에서 이것을 테스트하려면 원격 디버깅을 사용하고 있는지 확인하여 콘솔에 기록 된 내용을 볼 수 있습니다.

1. 크롬을 연 뒤에 우리의 날씨PWA로 들어갑니다.
2. 개발자 도구를 열어서 Console로 이동합니다.
3. 우측상단에 설치버튼을 클릭합니다. 
   * 설치버튼이 사라지는 것을 볼 수 있습니다!
   * 설치하겠냐는 모달이 나옵니다!
4. 취소를 눌러봅니다.
   *  "*User dismissed the A2HS prompt*" 가 console에 보일것입니다.
     * 이때 {outcome: "dismissed", platform: ""}
   * 설치 아이콘이 다시 보여집니다.
5. 다시 설치아이콘을 누르고 이번에는 설치버튼을 누릅니다.
   *  "*User accepted the A2HS prompt*" 가 console에 보일것 입니다.
     * 이때 {outcome: "accepted", platform: "web"}
   *  "*Weather App was installed*" 가 console에 보일것입니다. 
   * 우리의 날씨앱이 일반적으로 찾을 수 있는 곳에 있는지 확인 합니다. 
     * EX) 데스크탑의 바탕화면 , 모바일의 메뉴 또는 바탕화면
6.  날씨 PWA 설치 완료확인!
   * 앱이 데스크톱의 앱 창 또는 모바일의 전체 화면에서 독립형 앱으로 열리는 지 확인합니다.



NOTE : localhost의 데스크톱에서 실행중인 경우 localhost는 보안 호스트로 간주되지 않으므로 설치된 PWA에 주소 배너가 표시 될 수 있습니다.

### iOS에서 설치가 제대로 작동되는지 확인하기

**저는 iOS를 쓰지 않아서 정확한 정보를 드릴 수 없어서 해당내용을 번역만 해서 드리겠습니다...**

iOS에서의 동작을 확인합시다. iOS 기기가있는 경우 해당 기기를 사용하거나 Mac을 사용하는 경우 Xcode와 함께 제공되는 iOS 시뮬레이터를 사용해보십시오.

1. 사파리 브라우저에서 날씨PWA로 들어갑니다.
2. Share 버튼을 클릭합니다.
3. 오른쪽으로 스크롤하여 홈 화면에 추가 버튼을 클릭하십시오.
   * 타이틀, URL, 아이콘이 맞는지 확인하세요
4. Add를 클릭합니다.
   * 앱의 아이콘이 화면에 추가될 것입니다.
5. 날씨 PWA 를 홈화면에서 실행 시켜봅시다.
   * 앱이 fullscreen으로 실행될 것 입니다.



### 보너스 정보

#### 홈화면에서 앱이 실행되는것을 감지하기

미디어 쿼리를 사용하여 display-mode 로 앱시작 방법에 따라 다른 스타일을 적용하거나 javascript를 이용해서 스타일을 적용할 수 있다. 아래의 코드를 참고하자

```css
/* css */
@media all and (display-mode: standalone) {
  body {
    background-color: yellow;
  }
}
```

```javascript
/* javascript */
if (window.matchMedia('(display-mode: standalone)').matches) {
  console.log('display-mode is standalone');
}
```

사파리에서 확인하기

```javascript
/* javascript - 사파리 */
if (window.navigator.standalone === true) {
  console.log('display-mode is standalone');
}
```

 

#### 설치삭제(Uninstall)하기

앱이 이미 설치되어 있으면 beforeinstallevent가 시작되지 않으므로 개발 중에 앱이 여러 번 설치 및 제거되어 모든 것이 예상대로 작동하는지 확인해야합니다.



##### 안드로이드

1. 앱서랍에서 날씨앱을 찾습니다.
2. 아이콘을 선택한뒤
3. 삭제를 누릅니다.
4. 읭 너무간단...

##### 크롬

1. 설치한 날씨앱에 들어갑니다.
2. 점3개 옵션에서 제거를 누릅니다.
3. 삭제됩니다.
4. 읭 너무 간단행

##### 맥os와 윈도우

1. 새로운 탭을 열고 *chrome://apps*. 에 들어갑니다.
2. 날씨PWA를 찾아서 
3. 지웁니다
4. 읭 너무 쉽넹 
5. 같은 방법으로 크롬에서도 지울 수 있습니다.





## 👉PWA 완료!!!!

### 축하드립니다!!

우리는 간단한 PWA 앱을 성공적으로 만들었습니다.!!

웹 앱 매니페스트를 추가하여 설치할 수 있도록하고 PWA가 항상 빠르고 안정적이되도록 서비스 워커를 추가했습니다. DevTools를 사용하여 앱을 감사하고 사용자 경험을 향상시키는 데 도움이되는 방법을 배웠습니다.



앞으로 우리는 progressive답게 점진적으로 발전해야합니다. 

[웹푸시 노티피케이션](https://codelabs.developers.google.com/codelabs/push-notifications/index.html#0)을 추가하거나 [높은 퍼포먼스를 내는 서비스워커](https://developers.google.com/web/fundamentals/primers/service-workers/high-performance-loading) 를설계하는 등  할것이 많습니다. 



앞으로도 더 많은 발전이 있길 빕니다

