## 1/8 - 1일차
# React-query

<aside>
💡 1. fetching
2. caching
3. 서버 데이터와의 동기화 지원

</aside>

### 1. 캐싱(Caching)

> 캐싱을 통해 **`반복적인 비동기 데이터 호출을 방지`**, 불필요한 API 콜을 줄여 **`서버에 대한 부하는 줄이는 결과`**
> 

<aside>
💡 react-query 에서 최신의 데이터 → fresh한 데이터, 기존의 데이터를 stale한 데이터라고 한다.

</aside>

### 언제 데이터 갱신해야할까?

1. 화면을 보고 있을때 → refetchOnWindowFocus
    
    (브라우저에 포커스가 들어온 경우)
    
2. 페이지의 전환이 일어났을 때 → refetchOnMount
    
    (새로운 컴포넌트 마운트가 발생한 경우)
    
3. 페이지 전환 없이 이벤트가 발생한 데이터를 요청할 때 → refetchOnReconnect
    
    (네트워크 재연결이 발생한 경우)
    

### staleTime, cacheTime

### staleTime

- 데이터가 fresh → stale 상태로 변경되는데 걸리는 시간
    
    (ex. 나이 데이터 받아오면 1년동안 바뀌지 않음 → staletime)
    
- fresh 상태일 때 Refetch 트리거(위의 3가지 경우)가 발생해도 Refetch가 일어나지 않는다!
- 기본값이 0이므로 따로 설정하지 않으면 Refetch 트리거가 발생했을 때 무조건 Refetch가 발생한다.

### cacheTime

- 데이터가 inactive한 상태일 때 캐싱된 상태로 남아있는 시간
- 특정 컴포넌트가 unmount되면 사용된 데이터는 inactive 상태로 바뀌고, 이때 데이터는 cacheTime만큼 유지
- cacheTime 이후 데이터는 가비지 콜렉터로 수집되어 메모리에서 해제
- cacheTime이 지나지 않았는데 해당 데이터를 사용하는 컴포넌트가 다시 mount되면, 새로운 데이터를 fetch 해오는 동안 캐싱된 데이터를 보여줌

⇒ 캐싱된 데이터를 계속 보여주는게 아니라 fetch하는 동안 **`임시`**로 보여주는 것

## 2. Client 데이터와 Server 데이터 간의 분리

client에서 관리하는 데이터와 server에서 관리하는 데이터가 분리될 필요성을 느낀다.

- client data : 모달 관련 데이터, 페이지 관련 데이터…
- server data : 사용자 정보, 비즈니스 로직 관련 데이터…

⇒ 비동기 API 호출을 통해 불러오는 데이터들을 server데이터라 할 수 있다.

```jsx
const { data, isLoading } = useQueries(
	['unique-key'],
	() => {
		return api({
			url: URL,
			method: 'GET',
		});
	},
	{
		onSuccess: (data) => {
			// data로 이것저것 하는 로직
		}
	},
	{
		onError: (error) => {
			// error로 이것저것 하는 로직
		}
	}
)
```

⇒ onSuccess와 onError 함수를 통해 fetch 성공과 실패에 대한 분기를 간단하게 구현 가능

→ server 데이터를 불러오는 과정에서 구현해야 할 추가적인 설정들을 진행할 필요x

<aside>
💡 client 데이터는 상태관리 라이브러리가 관리하고, server 데이터는 react-query가 관리하는 구조

</aside>

## 3. 대표적인 기능

> GET - useQuery
PUT, UPDATE, DELETE - useMutation
>

## 1/9 - 2일차
# 패키지 매니저

모듈 없이 구현하려면 힘드니까.. 다른 사람이 미리 해놓은 거 갖다 쓰자!

## 5.1 npm | 패키지 매니저

> npm에 업로드된 노드 모듈을 패키지라고 한다.
모듈이 다른 모듈 사용할 수도 있다 ⇒ 의존 관계
> 

**`라이브러리 > 패키지 > 모듈`**

- 라이브러리 :  여러 패키지와 모듈들을 모아놓은 것
- 패키지 : 특정 기능과 관련된 여러 모듈을 한 **폴더** 안에 넣어 관리
- 모듈 : 함수, 변수, 클래스를 모아놓은 것. 하나의 **파일** 안에 모여있는 것이라고 할 수 있음

역할

1. 온라인 플랫폼. 누구나 노드 패키지를 만들고, 업로드, 공유할 수 있음.
2. 명령 줄 인터페이스. 온라인 플랫폼과 상호 작용하기 위해 명령 줄 인터페이스를 사용하며 패키지 설치 및 제거가 가능

### npm 대체제 | [yarn](https://engineering.fb.com/2016/10/11/web/yarn-a-new-package-manager-for-javascript/)

npm과 호환하면서 속도나 안정성 측면에서 npm보다 향상됨.

`npm install yarn —global`

`brew install yarn`

**그동안의 문제점**

1. 서로 다른 시스템 및 사용자 간의 종속성을 설치할 때 일관성 문제
2. 종속성을 가져오는 데 걸리는 시간
3. npm 클라이언트가 이러한 종속성 중 일부에서 코드를 실행하는 방식의 보안 문제

**npm 확장 시도**

1. package.json을 수동으로 npm install 해가며 실행 → 지속성의 문제
2. 모든 node_modules 깃 레포에 체크인 → 데이터양이 너무 많고 오래 걸림
3. 전체 node_modules를 압축하고 내부 CDN으로 업로드하여 다운로드 권장 → 빌드하기 위한 액세스가 또 필요함
4. 종속성 버전을 건드리지 않는 npm shrinkwrap → 거대한 JSON 파일로 정렬되지 않은 key들이 있어  완화시키려면 또 많은 줄의 JS 코드가 필요함

**새 클라이언트 구축**

yarn은 npm 레지스트리와의 호환성을 유지하면서 기존 워크플로를 대체하는 새로운 패키지 관리자.

노드에서 종속성은 node_modules 디렉토리 내에 배치되는데, 이 파일 구조는 중복 종속성이 함께 있기 때문에 실제 종속성 트리와 다를 수 있다. 즉, 설치된 dependencies 종속성에 따라 디렉토리 구조가 다를 수 있다. 이러한 점이 버그를 찾아내는데 오랜 시간이 걸리는 원인이 될 수 있다.

yarn은 안정적인 설치 알고리즘과 잠금 파일을 사용해서 버전 관리와 같은 문제를 해결한다. 

1. 해결 : yarn은 레지스토리에 대한 요청을 만들고 dependencies를 재귀적으로 조회하면서 종속성을 해결한다.
2. 가져오기 : 글로벌 캐시 디렉토리에서 필요한 패키지가 이미 다운되었는지 확인한다. 다운이 안되어 있다면, 패키지의 tarball을 가져와 글로벌 캐시에 배치하므로 오프라인에서도 작업할 수 있고, 종속성을 두 번 이상 다운할 필요가 없다.
3. 연결 : 전역 캐시에서 필요한 모든 파일을 로컬 node_modules 디렉토리로 복사하여 모두 연결한다.

yarn.lock 잠금파일로 매번 동일한 패키지를 얻을 수 있다.

### npm vs yarn

**패키지 설치 프로세스를 처리하는 방법**

- npm은 패키지를 한번에 하나씩 순차적으로 설치, 자동으로 패키지에 포함된 다른 패키지 코드 실행 ⇒ 보안에 취약
- yarn은 여러 패키지를 동시에 가져오고 설치하도록 최적화, yarn.lock or package.json에 있는 파일 만을 설치

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f66b956b-8ea9-4ee4-8985-ad3cd2c6a7e4/Untitled.png)

## 5.2 package.json으로 패키지 관리

사용할 패키지의 고유한 버전을 기록해두어야 한다. 같은 패키지라도 버전별로 기능이 다를 수 있으므로 버전까지 기록해놓아야 한다. 

> 설치한 패키지의 버전을 관리하는 파일
> 

“노드 프로젝트를 시작하기 전에는 폴더 내부에 무조건 package.json부터 만들고 시작해야 한다.”

### 라이선스

ISC, MIT, BSD : 사용한 패키지와 라이선스만 밝히면 자유롭게 사용 가능

아파치 : 사용은 자유롭지만 특허권에 대한 제한 포함

GPL : 소스 코드도 공개해야 함

scripts 부분은 npm 명령어를 저장해두는 부분.

콘솔에 npm run test, npm run build하면 스크립트가 실행됨.

```tsx
{
	"scripts":{
		"test" : 
		"build" : 
	}
}
```

start 명령어에 node [파일명]을 해두고 `npm start` 실행시킴. start나 test는 run을 안 써도 됨.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/05e277cf-1fd4-4c6c-b258-cf02ce0e9e58/Untitled.png)

8개의 패키지를 추가했고, 10 패키지들의 취약점을 2초 만에 검사를 했습니다.

만약 취약점을 발견하면 found ~~~ 이렇게 뜨고, npm audit으로 취약점을 검사할 수 있음.

npm audit fix를 입력하면 npm이 스스로 수정할 수 있는 취약점을 스스로 수정해줌.

 **`—save 옵션`**

원래는 dependencies에 패키지 이름을 추가하는 옵션이지만, npm@5부터는 알아서 해주니까 안 붙여도 되긴 함.

→ dependencies

**`—save-dev 옵션 <-D>`**

실제 배포 시에는 사용되지 않고 개발 중에만 사용되는 패키지

→ devDependencies

**`전역설치 —global 옵션 <-g>`**

시스템 환경 변수에 등록되어 있으므로 전역 설치한 패키지는 **콘솔 명령어로 사용가능**.

맥은 관리자 권한이 필요하기 때문에 `sudo npm install —global [이름]`

전역 설치 패키지는 package.json에 기록되지 않음.

그래서 package.json에 기록되지 않는게 불편한 개발자는

`npm install —save-dev [이름]`

`npx [이름] node_modules`

첫 번째 줄로 devDependencies에 기록 후, npx 명령어를 붙여 실행하면 전역 설치한 것과 같이 명령어로 사용 가능하다. 또, package.json에 기록되었기에 버전 관리도 용이하다.

## 5.3 패키지 버전 이해하기

Semver방식 (Semantic Versioning)

### 여기서 잠깐! semantic 짚고 넘어가자

[semantic이란?](https://www.notion.so/semantic-698f4918c460483087b880fa9a0d8bdd?pvs=21)

### 1.0.3

- 첫 번째 자리 1 - major 버전
    - 0이면 초기 개발, 1부터 정식 버전.
    - 하위 호환이 안 될 정도로 패키지 내용이 수정되었을 때
    - 1→2로 업데이트 하면 에러 확률 많다..
- 두 번째 자리 0 - minor 버전
    - 하위 호환이 되는 기능 업데이트
- 세 번째 자리 3 - patch 버전
    - 기존 기능에 문제가 있어(버그) 수정한 것을 내놓았을 때

^ : minor 버전까지만 설치하거나 업데이트 

ex) ^1.1.1 | 1.1.1 ~ 2.0.0 미만

~ : patch버전까지만 설치하거나 업데이트

@latest : 안정된 최신 버전 

@next : 가장 최근 배포판(베타 버전도)

## 5.4 기타 npm 명령어

## 5.5 패키지 배포하기