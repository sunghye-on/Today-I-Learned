# 캐시 스토리지 API

## 캐시 스토리지(CacheStorage)란?

* 개발자가 완전히 제어 할 수 있는 새로운 형태의 캐싱 레이어

* 이전 기술과는 달리 캐시 생성 및 관리를 위한 기본적인 API를 직접 제공한다. 
* 서비스워커와 결합하여 캐시에서 뭘 삭제할지, 어떤 응답이 캐시로 부터 반환되는지, 프로그램을 직접 제어할 수 있다.





#### 서비스워커의 install 이벤트 

* 서비스 워커가 처음 등록된 직후 그리고 이벤트가 활성화되기 직전에 단 한번 발생하는 이벤트
* 서비스 워커가 페이지를 제어히고 fetch이벤트를 수신하기전 오프라인화 가능한 모든 파일을 캐싱힐 기회를 얻는다





## 캐시스토리지 요청저장

먼저 install 이벤트를 위해 이벤트를 추가하자

* waitUntil(비동기이벤트)을 사용하여 index-offline.html파일을 가져와 캐시에 저장할 때까지 install이벤트를 연기한다. 
* 또한 waitUntil함수에서 caches.open을 호출하여 캐시명을 전달 받고 cache.add로 원하는 파일(index-offline.html)을 저장한다. 
* 작업이 완료되면 index-offline.html은 서비스워커가 관리하는 모든 페이지에서 사용이 가능하다.

```javascript
self.addEventListener("install",function(event){
    event.waitUntil(
        caches.open("gih-cache").then(function(cache){
            return cache.add("/index-offline.html");
        })
    );
});
```



## 캐시스토리지로 부터 요청 받아오기

* install 이벤트 아래에 이제 fetch 이벤트를 생성해준다.
* fetch함수의 비슷한 모습을 보이고 있다.
* 대신 웹에서 받아오는 것이 아닌 caches.match를 이용해 캐시중에 index -offline.html을 찾아서 가져온다.

```javascript
self.addEventListener('fetch', function(event) {
    event.respondWith(
        fetch(event.request).catch(function() {
            return caches.match('/index-offline.html');
        })
    );
});
```





### match 메소드

#### match(request [,options]);

match는 주어진 requestㅇ 대하여 캐시로 부터 response객체를 반환한다.

모든 캐시에서 match를 검색하거나 특정 캐시 객체에서 호출될 수 있다.

```javascript
//모든 캐시에서 일치하는 request를 검색
caches.match("logo.png");
//특정 캐시에서 일치하는 request를 검색
caches.match("my-cahce").then(function(cache){
    return cache.match("logo.png");
})
```

match의 첫번째 인자는 fetch와 마찬가지로 검색하고자 하는 request 객체나 URL이다.

match의 두번째 인자는 선택적 option으로 match는 캐시에서 가장 처음 검색된 response 혹은 항목이 없는 경우 undefined로 프로미스를 반환한다. 즉 match는 response를 찾지 못해도 reject을 빈횐히지 않음 그러므로 response가 존재하는지 확실하지 않을때 response를 반환하기전에 확인해야한다.

```javascript
caches.match("/logo.png").then(function(response){
   if (response){
       return response;
   } 
});
```



## offline 페이지에 스타일 캐시 저장

css와 로고 이미지들을 install에서 모두 불러 보았다. 

이렇게 된다면 서비스워커의 설치를 매우 느리게 만든다.

```javascript
self.addEventListener('install', function(event) {
    event.waitUntil(
        caches.open('gih-cache').then(function(cache) {
            return cache.add('/index-offline.html').then(function(){
                return cache.add(
                    "https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css"
                );
            }).then(function(){
                return cache.add("/css/gih-offline.css");
            }).then(function(){
                return cache.add("/img/jumbo-background-sm.jpg");
            }).then(function(){
                return cache.add("/img/logo-header.png");
            });
        })
    );
});

```

아래와 같은 방식으로 cache.add 가 아닌 cache.addAll()을 호출해서 URL 배열을 넘거준다. 

```javascript
var CACHE_NAME = "gih-cache";
var CACHE_URL = [
    "/index-offline.html",
    "https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css",
    "/css/gih-offline.css",
    "/img/jumbo-background-sm.jpg",
    "/img/logo-header.png"
];

self.addEventListener('install', function(event) {
    event.waitUntil(
        caches.open(CACHE_NAME).then(function(cache) {
            return cache.addAll(CACHE_URL);
        })
    );
});
```



## 각각 요청에 올바른 응잡 매칭하기

소스들을 캐싱했지만 페이지는 여전히 index-offline만을 보여준다. 우리가 캐싱한 것을 fetch핸들러로 불러보자

* 기존 구조와 비슷하게 네트워크로부터 요청을 가져오고 실패하면 catch함수를 실행한다.
* catch함수는 캐시안에 저장된 요청을 매칭하는 것으로 시작한다. 
* 위에서 언급한 match는 리젝은 반환하지 않으므로 if(response)를 사용하여 캐시가 있는지 확인 

```javascript
self.addEventListener('fetch', function(event) {
    event.respondWith(
        fetch(event.request).catch(function() {
            return caches.match(event.request).then(function(response) {
                if (response) {
                    return response;
                } else if (event.request.headers.get('accept').includes('text/html')) {
                    return caches.match('/index-offline.html');
                }
            });
        })
    );
});
```

  