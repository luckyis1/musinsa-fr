# musinsa-fr

## Project setup
npm install

### 구동방법
npm run serve


vue를 선택하여 개발한 이유는 기본적으로 가볍고 빠르게 프로토타입 형태를 만들기 쉬운 작고 가벼운 크기의 프레임워크인 것을 포함하여 과제의 내용인 리스팅을 구현하기에 적합한 양방향 데이터 바인딩이 지원되는 프레임워크이기 때문입니다.
프로젝트의 스캐폴딩을 구현해주는 vue-cli를 사용하여 보다 빠르게 결과를 구현하기 위한 선택이었습니다.

http request를 위한 라이브러리로 axios를 선택했습니다.
Es7부터 기본 제공되는 fetch는 async/await 비동기 처리마저 기본적으로 제공되지만 ie에서 동작하지 않기때문에 polyfill을 추가하기보다 조금 더 안정적인 axios를 사용하는게 낫다고 판단했습니다.

