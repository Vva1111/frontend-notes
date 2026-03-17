# Claude Code MCP 서버 설정 가이드

  

## MCP 서버 설정 방법

  

Claude Code에서 MCP 서버를 설정하는 방법은 두 가지가 있습니다:

  

### 1. 글로벌 설정 (추천)

- **적용 범위**: 모든 Claude Code 프로젝트에 적용

- **설정 방법**: `claude mcp add-json` 명령어 사용

- **장점**: 한 번 설정하면 모든 프로젝트에서 사용 가능

- **단점**: 프로젝트별 커스터마이징 제한

  

### 2. 프로젝트별 설정

- **적용 범위**: 특정 프로젝트에만 적용

- **설정 방법**: 프로젝트 루트에 `.mcp.json` 파일 생성

- **장점**: 프로젝트별 맞춤 설정 가능

- **단점**: 프로젝트마다 개별 설정 필요

  

## 글로벌 MCP 서버 설정 방법

  

다음 명령어들을 순서대로 실행하여 글로벌 MCP 서버를 설정하세요:

  

```bash

# 1. Memory 서버 추가

claude mcp add-json memory '{"command": "npx", "args": ["-y", "@modelcontextprotocol/server-memory"]}'

  

# 2. Think 서버 추가 (Anthropic 공식)

claude mcp add-json think '{"command": "npx", "args": ["-y", "@anthropic-ai/mcp-server-think"]}'

  

# 3. Sequential Thinking 서버 추가

claude mcp add-json sequential-thinking '{"command": "npx", "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]}'

  

# 4. Filesystem 서버 추가

claude mcp add-json filesystem '{"command": "npx", "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]}'

  

# 5. Git 서버 추가

claude mcp add-json git '{"command": "npx", "args": ["-y", "@modelcontextprotocol/server-git", "--repository", "."]}'

```

  

### 설정 확인

  

```bash

# 설정된 MCP 서버 목록 확인

claude mcp list

  

# 특정 서버 상세 정보 확인

claude mcp get <server-name>

```

  

## MCP 서버별 특징 및 기능

  

### 1. Memory 서버

- **패키지**: `@modelcontextprotocol/server-memory`

- **기능**: 대화 기억 및 컨텍스트 관리

- **특징**:

  - 이전 대화 내용 기억

  - 장기 컨텍스트 유지

  - 세션 간 정보 연속성 제공

- **사용 예시**: 프로젝트 히스토리, 사용자 선호도 기억

  

### 2. Think 서버 (Anthropic 공식)

- **패키지**: `@anthropic-ai/mcp-server-think`

- **기능**: 고급 추론 및 사고 과정 지원

- **특징**:

  - 복잡한 문제 해결 능력 향상

  - 단계별 논리적 사고 과정

  - Anthropic의 공식 지원

- **사용 예시**: 복잡한 알고리즘 설계, 아키텍처 결정

  

### 3. Sequential Thinking 서버

- **패키지**: `@modelcontextprotocol/server-sequential-thinking`

- **기능**: 단계별 사고 과정 구조화

- **특징**:

  - 순차적 문제 해결 접근

  - 체계적 사고 과정 지원

  - 복잡한 작업의 단계별 분해

- **사용 예시**: 프로젝트 계획, 디버깅 과정

  

### 4. Filesystem 서버

- **패키지**: `@modelcontextprotocol/server-filesystem`

- **기능**: 향상된 파일 시스템 작업

- **특징**:

  - 고급 파일 탐색 및 조작

  - 대용량 파일 처리

  - 디렉토리 구조 분석

- **사용 예시**: 코드베이스 분석, 파일 리팩토링

  

### 5. Git 서버

- **패키지**: `@modelcontextprotocol/server-git`

- **기능**: 고급 Git 작업 지원

- **특징**:

  - 복잡한 Git 명령어 실행

  - 커밋 히스토리 분석

  - 브랜치 관리 자동화

- **사용 예시**: 코드 리뷰, 버전 관리 최적화

  

## 서버 관리 명령어

  

### 추가 명령어

```bash

# 서버 제거

claude mcp remove <server-name>

  

# 서버 상세 정보 조회

claude mcp get <server-name>

  

# Claude Desktop에서 서버 가져오기

claude mcp add-from-claude-desktop

```

  

### 프로젝트별 설정 파일 예시

  

만약 특정 프로젝트에만 MCP 서버를 설정하고 싶다면, 프로젝트 루트에 `.mcp.json` 파일을 생성하세요:

  

```json

{

  "mcpServers": {

    "memory": {

      "command": "npx",

      "args": ["-y", "@modelcontextprotocol/server-memory"]

    },

    "think": {

      "command": "npx",

      "args": ["-y", "@anthropic-ai/mcp-server-think"]

    },

    "sequential-thinking": {

      "command": "npx",

      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]

    },

    "filesystem": {

      "command": "npx",

      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]

    },

    "git": {

      "command": "npx",

      "args": ["-y", "@modelcontextprotocol/server-git", "--repository", "."]

    }

  }

}

```

  

## 주의사항

  

1. **Node.js 필요**: MCP 서버는 Node.js 환경에서 실행됩니다.

2. **인터넷 연결**: 서버 패키지 다운로드 시 인터넷 연결이 필요합니다.

3. **Claude Code 재시작**: 설정 변경 후 Claude Code를 재시작해야 적용됩니다.

4. **권한 설정**: 일부 서버는 추가 권한이 필요할 수 있습니다.

  

## 문제 해결

  

### 서버 연결 실패 시

```bash

# 서버 목록 확인

claude mcp list

  

# 서버 제거 후 재추가

claude mcp remove <server-name>

claude mcp add-json <server-name> '<json-config>'

```

  

### 성능 문제 시

- 불필요한 서버 제거

- Claude Code 재시작

- 시스템 리소스 확인

  

---

  

이 가이드를 따라하면 Claude Code에서 MCP 서버를 효과적으로 활용할 수 있습니다. 각 서버의 특징을 이해하고 프로젝트에 맞게 선택하여 사용하세요.