# DJango_Vue_Project



## Django API Server 

```bash
$python -m venv venv  //가상환경 만들기 
$source venv/Script/activate //가상환경 바라보기

//Git 환경
$touch .gitignore
$git init
...

//필요한 패키지 설치

$pip install django==2.1.15
$pip install djangorestframework

$pip install django-rest-auth
$pip install django-allauth

// 패키지 관리

$pip freeze > requirements.txt
```









## Live



`request.data` vs `request.POST`

`form-data`와 `JSON-data`를 보냈을 때 차이점

- `request.data`는 Form-data와  JSON 둘다 잡음
- `request.POST`는 Form-data만 잡을 수 있음



**Authentication** 

1. session 인증 방식

- 서버에 Template까지 붙어있어야 사용가능 (같은 포트에서 데이터 주고받을 때만 가능)

2. Django | Vue 로 나눠지는 상황일 때,

   - `Token` 이용

   `django-rest-auth` :login logout

   `django-rest-auth` + `django-allauth` : login, logout, signup

   `Header`에 `Authorization` : `TOKEN {{KEY}}` 형태로 넣어서 보내주기
   
   - 로그아웃 시
   
     Token없이 보내도 로그아웃이 완료되는 현상이 발생



 

비동기 요청과 응답에 관하여

```javascript
const xhr = new XMLHttpRequest()
xhr.open('GET','http://localhost:8000/articles')
xhr.send()
```

Access to XMLHttpRequest at 'http://localhost:8000/articles' from origin 'https://vuejs.org' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.

### CORS란?

- 결론 : 브라우저가 너무 친절하다 

- 서버에서는 Response를 날렸지만.. 브라우저가 막은 케이스.

- 비동기 요청의 응답의 헤더에 `Access-Control-Allow-Origin` 가 필요함!



cors설치

`$ pip install django-cors-headers`



`settings.py`

```python
INSTALLED_APPS = [
    ...
    'corsheaders',
    ...
]
MIDDLEWARE = [  # Or MIDDLEWARE_CLASSES on Django < 1.10
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
]
CORS_ORIGIN_ALLOW_ALL = True
```









# Vue_For_Django

```bash
$vue create vue-for-django

$vue add router

// Ajax 설치
$npm i axios
```





**Login | Logout | Signup**



**쿠키설정**

```bash
//vue cookies 설치
$npm i vue-cookies
```

```javascript
// Apply
import Vue from 'vue'
import VueCookies from 'vue-cookies'
Vue.use(VueCookies)
```

```javascript
//1. 저장 | {'auth-token' : key }  KEY:VALUE
this.$cookies.set('auth-token', key)

//2. 제거 'auth-token' 키값 제거.
this.$cookies.remove('auth-token')
```

```javascript
//3. 헤더 정보 넣기
const requestHeader = {
        headers: {
          //2. $cookies.get을 통해 'auth-token'이라는 이름으로 저장된 토큰 값 가져오기
          Authorization: `Token ${this.$cookies.get('auth-token')}`
        }
      }
//3. 비동기 요청으로 logout 요청 보내기 
// axios (a,b,c) 위치인자 a,b,c에대하여 
// a : URL  , b : Data,  c: Header 이므로, b를 null로 넣어두고 header를 c자리에 넣어야함.
axios.post(`${SERVER_URL}/rest-auth/logout/`, null, requestHeader)
    .then( () => {
    this.$cookies.remove('auth-token')
    this.isLogin = false
    this.$router.push('/')
})
```



logout

컴포넌트는 사실 이벤트를 들을 때 밑에서 올라오는 이벤트를 듣게 되어있음

`@click.native`를 먹여야 해당 컴포넌트 안에서 먹음.





**vue router guard**

> 사용자가 URL에 직접 접근할 때. 방어



**Vue LifeCycle Hook**

`new Vue()`

- `beforeCreate () {}`

`Observe data & initialize events`

- `created () {}`

  ```javascript
  this.$cookies.isKey('auth-token') // Vue 인스턴스가 생성되기 전에도 cookie는 존재한다.
  ```

`Compile Templates`

- `mounted () {}`







## VUEX 적용

