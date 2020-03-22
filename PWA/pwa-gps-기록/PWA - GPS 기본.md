# PWA - GPS

## 브라우저의 지원

우선 우리의 페이지가 위치를 요청하기 위해서는 브라우저의 지원이 필요합니다. 현재 Chrome, Edge, Firefox, Safari, Opera 등 많은 브라우져가 최신기능을 지원하지만 IE에서는 몇가지 구성들과 씨름을 해야하는 상황이 올 것입니다. 우리 모두 크롬을 사용합시다~

또 하나 알아둬야 할 것은 Chrome 버전 50.0부터 위치정보는 SSL 계층 (HTTPS)을 구현하는 보안 연결을 통해서만 지원됩니다. 지리적 위치는 HTTP와 같은 비보안 출처에서는 작동하지 않습니다. 



## 사용자의 위치정보 얻기 및 확인하기

자바스크립트를 이용하여 사용자의 위치정보를 얻어 콘솔에 찍히도록하는 함수를 작성해봅시다!

```javascript
//위치정보를 알기 위해 권한을 요청하거나 브라우저의 지원가능성을 확인 한다.
function getLocation() {
  if(navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(showPosition);
  } else {
    console.log("위치정보를 지원하지 않는 브라우져입니다.");
  }
}
//위치정보를 loacation에 저장한다. 
function showPosition(position) {
  var location = {
    longitude: position.coords.longitude,
    latitude: position.coords.latitude
  }
  console.log(location)
}
//함수 실행
getLocation();
```



이제 우리의 페이지에서 실행 시켜봅니다! 그후 F12혹은 요소검사를통한 개발자도구로 들어간 뒤 console.log를 확인 해봅니다 location에 longitude와 latitude가 잘 찍힌 것을 볼 수 있습니다.

이제 확인 해봅시다 https://www.geoplaner.com/ 사이트에서 dd.ddddd에서 적절히 입력하고 확인 하면 비슷하게 나오는 것을 알 수 있습니다.



## 오류

위치정보를 자져오는데에 생각보다 다양한 오류들이 있다.

* PERMISSION_DENIED
  * 사용자가 권한을 거절한 경우에 생기는 오류이다.
* POSITION_UNAVAILABLE
  * gps장치가 없거나사용자의 위치를 받아올 수 없을때 생기는 오류이다.
* TIMEOUT
  * 연결이 늦어져서 시간이 오래걸리면 생기는 오류이다.
* UNKNOWN_ERROR
  * 알수없는 오류가 발생한 상황이다.



## 지도를 사용하는 방법

지도를 사용하여 내사이트에 지도로 표시하는 방법도 있습니다. 바로 구글 지도를 이용하는 것인데요

구글지도에는 매개 변수를 직접 추가하고 지도에 좌표를 표시할 수 있는 고유한 URL이 있습니다.  몰론 API키도 필요합니다 아래의 코드는 위도와 경도를 사용하여 정적이미지를 사용하여 지도에서 위치를 포시했습니다.

```javascript
function showInMap(pos) {
    var latlon = pos.coords.latitude + "," + pos.coords.longitude;

    var img_url = "https://maps.googleapis.com/maps/api/staticmap?center=
    "+latlon+"&zoom=14&size=400x300&sensor=false&key=YOUR_:KEY";
    var map = document.querySelector("mapholder");
    map.innerHTML = "<img src='"+img_url+"'>";
}
```



## Geolocation 객체

네비게이터 클래스에는 조작할 수 있는 두가지 더 재미있는 메소드가 있습니다. 

첫번째는 **watchPosition()**메소드로 사용자의 위치정보를 전달하는 하나의 매개변수를 허용한뒤 사용자를 감사히고 사용자를 추적합니다.자동차의 이동같은곳에서 사용할 수 있다.

두번째는 watchPosition()을 중지하는 **clearWatch()**입니다. 



