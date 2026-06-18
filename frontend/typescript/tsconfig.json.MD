 📦 TypeScript 모듈 시스템 완벽 이해 프로세스 (by 슬기)
 ====================================================================================
 1. [module] 옵션은 내가 쓴 최신 import/export 구문을 최종 결과물에서 어떤 형태로 '출력'할지 결정함.
    - "CommonJS"로 출력 시: import가 require로 바뀜. 단, 동적 구조라 '트리 쉐이킹(가지치기)' 최적화 불가능.
    - "ESNext"로 출력 시: import/export 문법이 정적 구조 그대로 유지되며, '트리 쉐이킹' 최적화 가능.

 2. [호환성 에러 발생 시점]: 구버전(CommonJS) 라이브러리를 import 문으로 부르면 발생.
    - ESM은 내부적으로 '.default' 값을 찾으려 하지만, CommonJS 라이브러리에는 export default가 없어 undefined 에러가 남.

 3. [esModuleInterop: true]: 위 에러를 막기 위해 CommonJS 모듈을 '.default'가 먹히는 객체로 강제 래핑해 줌.
    - 이제 구버전 라이브러리도 모던한 import 방식으로 자유롭게 사용 가능.

 4. [importHelpers: true]: esModuleInterop이 주입하는 수많은 호환성 헬퍼 함수들을 
    - 소스코드에 직접 다 때려 박지 않고, 'tslib' 패키지에서 import 하도록 유도하여 최종 번들 용량을 최적화함.

 5. [결론]: esModuleInterop은 module 옵션(최종 출력 형태)이 무엇이든 간에, 
 - **"개발자가 소스 코드에서 통일된 최신 import 문법을 쓰면서도 
 - 옛날 CommonJS 라이브러리들을 아무 에러 없이 가져다 쓸 수 있게 해주는 마법의 래퍼(Wrapper)"**이다.
 - 만약 module: "CommonJS" 이고 esModuleInterop: false 인 상태에서 아래 코드를 작성하면:
 - import React from 'react'; // React는 옛날 CommonJS 방식으로 만들어진 라이브러리
 - TypeScript: "야, 네가 나중에 코드를 require로 바꾸라고 설정(module: "CommonJS")한 건 알겠는데, 
 - 지금 네가 쓴 문법(import default) 자체가 저 옛날 react 모듈이랑 규격이 안 맞아! 
 - react에는 default라는 게 없다고! 빨간 줄 딱 대! 아니면 esModuleInterop: true 로 바꿔서 호환성 래핑 되도록 해"

 6. "skipLibCheck": true, // 라이브러리의 타입검사는 스킵
 7. "forceConsistentCasingInFileNames": true // 윈도우(대소문자 구분X)와 배포서버/깃허브(리눅스, 대소문자 구분O) 간의 파일명 인식 차이로 인한 빌드 에러를 방지
