# 입력 템플릿 v1 확정안

> 작성일: 2026-03-14
> 버전: v1.0 (확정)
> 담당: CEO (클로이)
> 상태: 완료
> 관련 이슈: OPE-19

---

## 1. 개요

**목적:** 최소 입력으로 초안 생성 후, 추가 입력으로 고도화가 가능한 구조 확정

**두 가지 트랙:**
1. **무자료 입력 트랙** - 최소 정보로 빠른 초안 생성
2. **자료 입력 트랙** - 상세 정보로 고품질 산출물 생성

---

## 2. 무자료 입력 트랙 (Quick Draft)

### 2-1. 필수 입력값 (5개)

| 필드명 | 타입 | 제약조건 | 설명 |
|--------|------|----------|------|
| `service_name` | string | 2-50자 | 서비스/제품명 |
| `problem_summary` | string | 10-200자 | 해결하려는 핵심 문제 |
| `target_customer` | string | 2-50자 | 주요 고객군 |
| `solution_summary` | string | 10-200자 | 우리 해결 방식 요약 |
| `differentiation` | string | 10-200자 | 핵심 차별점 |

### 2-2. 선택 입력값 (3개)

| 필드명 | 타입 | 제약조건 | 설명 |
|--------|------|----------|------|
| `current_stage` | enum | ["idea", "mvp", "operating"] | 현재 단계 |
| `team_size` | number | 1-100 | 팀 규모 |
| `website_url` | string | URL 형식 | 웹사이트 (선택) |

### 2-3. 검증 규칙

```javascript
{
  "required": ["service_name", "problem_summary", "target_customer", "solution_summary", "differentiation"],
  "minLength": {
    "service_name": 2,
    "problem_summary": 10,
    "target_customer": 2,
    "solution_summary": 10,
    "differentiation": 10
  },
  "maxLength": {
    "service_name": 50,
    "problem_summary": 200,
    "target_customer": 50,
    "solution_summary": 200,
    "differentiation": 200
  }
}
```

---

## 3. 자료 입력 트랙 (Full Draft)

### 3-1. 기본 정보 (무자료 트랙 상속)

모든 무자료 입력값 + 아래 추가 필드

### 3-2. 시장/경쟁 정보

| 필드명 | 타입 | 제약조건 | 설명 |
|--------|------|----------|------|
| `market_size_tam` | string | 10-100자 | TAM (전체 시장 규모) |
| `market_size_sam` | string | 10-100자 | SAM (유효 시장 규모) |
| `competitors` | array | 1-5개 항목 | 주요 경쟁사 |
| `competitive_advantage` | string | 20-300자 | 경쟁 대비 우위 |

### 3-3. 사업성 정보

| 필드명 | 타입 | 제약조건 | 설명 |
|--------|------|----------|------|
| `revenue_model` | string | 20-200자 | 수익 모델 구조 |
| `pricing_strategy` | string | 10-100자 | 가격 전략 |
| `12month_target` | string | 10-100자 | 12개월 목표 지표 |

### 3-4. 실행 계획

| 필드명 | 타입 | 제약조건 | 설명 |
|--------|------|----------|------|
| `roadmap_3m` | string | 20-200자 | 3개월 로드맵 |
| `roadmap_6m` | string | 20-200자 | 6개월 로드맵 |
| `roadmap_12m` | string | 20-200자 | 12개월 로드맵 |
| `team_plan` | string | 20-200자 | 인력 계획 |

### 3-5. 예산 계획

| 필드명 | 타입 | 제약조건 | 설명 |
|--------|------|----------|------|
| `total_budget` | number | 100만-100억 | 총 사업비 |
| `budget_breakdown` | object | - | 항목별 예산 |
| `funding_plan` | string | 20-200자 | 정부지원금 사용 계획 |

### 3-6. 리스크/대응

| 필드명 | 타입 | 제약조건 | 설명 |
|--------|------|----------|------|
| `risks` | array | 1-5개 항목 | 주요 리스크 |
| `mitigation_strategies` | array | 1-5개 항목 | 대응 전략 |

---

## 4. JSON Schema 명세

### 4-1. 무자료 입력 스키마

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "QuickDraftInput",
  "type": "object",
  "required": ["service_name", "problem_summary", "target_customer", "solution_summary", "differentiation"],
  "properties": {
    "service_name": {
      "type": "string",
      "minLength": 2,
      "maxLength": 50,
      "description": "서비스/제품명"
    },
    "problem_summary": {
      "type": "string",
      "minLength": 10,
      "maxLength": 200,
      "description": "해결하려는 핵심 문제"
    },
    "target_customer": {
      "type": "string",
      "minLength": 2,
      "maxLength": 50,
      "description": "주요 고객군"
    },
    "solution_summary": {
      "type": "string",
      "minLength": 10,
      "maxLength": 200,
      "description": "우리 해결 방식 요약"
    },
    "differentiation": {
      "type": "string",
      "minLength": 10,
      "maxLength": 200,
      "description": "핵심 차별점"
    },
    "current_stage": {
      "type": "string",
      "enum": ["idea", "mvp", "operating"],
      "description": "현재 단계"
    },
    "team_size": {
      "type": "number",
      "minimum": 1,
      "maximum": 100,
      "description": "팀 규모"
    },
    "website_url": {
      "type": "string",
      "format": "uri",
      "description": "웹사이트 URL"
    }
  }
}
```

### 4-2. 자료 입력 스키마 (전체)

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "FullDraftInput",
  "type": "object",
  "allOf": [{ "$ref": "#/definitions/QuickDraftInput" }],
  "definitions": {
    "QuickDraftInput": {
      "type": "object",
      "required": ["service_name", "problem_summary", "target_customer", "solution_summary", "differentiation"],
      "properties": {
        "service_name": { "type": "string", "minLength": 2, "maxLength": 50 },
        "problem_summary": { "type": "string", "minLength": 10, "maxLength": 200 },
        "target_customer": { "type": "string", "minLength": 2, "maxLength": 50 },
        "solution_summary": { "type": "string", "minLength": 10, "maxLength": 200 },
        "differentiation": { "type": "string", "minLength": 10, "maxLength": 200 },
        "current_stage": { "type": "string", "enum": ["idea", "mvp", "operating"] },
        "team_size": { "type": "number", "minimum": 1, "maximum": 100 },
        "website_url": { "type": "string", "format": "uri" }
      }
    }
  },
  "properties": {
    "market_size_tam": { "type": "string", "minLength": 10, "maxLength": 100 },
    "market_size_sam": { "type": "string", "minLength": 10, "maxLength": 100 },
    "competitors": {
      "type": "array",
      "minItems": 1,
      "maxItems": 5,
      "items": { "type": "string", "minLength": 2, "maxLength": 50 }
    },
    "competitive_advantage": { "type": "string", "minLength": 20, "maxLength": 300 },
    "revenue_model": { "type": "string", "minLength": 20, "maxLength": 200 },
    "pricing_strategy": { "type": "string", "minLength": 10, "maxLength": 100 },
    "12month_target": { "type": "string", "minLength": 10, "maxLength": 100 },
    "roadmap_3m": { "type": "string", "minLength": 20, "maxLength": 200 },
    "roadmap_6m": { "type": "string", "minLength": 20, "maxLength": 200 },
    "roadmap_12m": { "type": "string", "minLength": 20, "maxLength": 200 },
    "team_plan": { "type": "string", "minLength": 20, "maxLength": 200 },
    "total_budget": { "type": "number", "minimum": 1000000, "maximum": 10000000000 },
    "budget_breakdown": {
      "type": "object",
      "properties": {
        "personnel": { "type": "number" },
        "development": { "type": "number" },
        "marketing": { "type": "number" },
        "operation": { "type": "number" },
        "other": { "type": "number" }
      }
    },
    "funding_plan": { "type": "string", "minLength": 20, "maxLength": 200 },
    "risks": {
      "type": "array",
      "minItems": 1,
      "maxItems": 5,
      "items": { "type": "string", "minLength": 10, "maxLength": 200 }
    },
    "mitigation_strategies": {
      "type": "array",
      "minItems": 1,
      "maxItems": 5,
      "items": { "type": "string", "minLength": 10, "maxLength": 200 }
    }
  }
}
```

---

## 5. 트랙별 출력물

### 5-1. 무자료 입력 트랙 출력

- **출력 A:** 1차 사업계획서 초안 (간략 버전)
- **출력 B:** 핵심 가설 리스트
- **출력 C:** 보완 권장 사항 (자료 입력 유도)

### 5-2. 자료 입력 트랙 출력

- **출력 A:** 완성형 사업계획서
- **출력 B:** 심사 관점 약점 리포트
- **출력 C:** AI 평가 점수 및 개선 포인트
- **출력 D:** 제출용 요약본 (1-2p)

---

## 6. 검증 체크리스트

- [x] 최소 입력으로 초안 생성 가능
- [x] 사용자에게 과도한 입력 부담 없음
- [x] 심사 기준 항목 누락 없음
- [x] 자동 보정 결과가 근거 기반
- [x] 민감정보 최소수집 원칙 지킴
- [x] JSON schema 유효성 검증 완료

---

## 7. 다음 액션

1. **개발팀 전달:** JSON schema 기반 입력 폼 개발
2. **프롬프트 엔지니어링:** 각 필드별 AI 프롬프트 최적화
3. **A/B 테스트:** 두 트랙별 전환율 비교
4. **v2 개선:** 파일럿 테스트 후 필드 추가/제거

---

*산출물 완료: 입력 템플릿 v1 확정안 + JSON schema 명세*
