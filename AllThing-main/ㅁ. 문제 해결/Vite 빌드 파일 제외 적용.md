Vite build 옵션 external 적용 Test

설정 포함하지 않았을 때
시간 - 111.2791s
용량 - 7.42MB

fileURLToPath(new URL('/src/views/guidePage/*', import.meta.url)), 설정 추가
시간 - 124.9136s
용량 - 7.42MB

const externalDirectory = []; 설정
시간 - 117.8834s
용량 - 7.42MB

const guidePagePathRegex = /src\/views\/guidePage\/.*/; 정규식 변경 방식 - 성공
시간 - 120.4995s
용량 - 7.33MB

정규식 변경 + 가이드 템플릿 페이지 예외 추가
시간 - 88.5779s
용량 - 6.57MB

-> 결국 정규식 사용 문제였음