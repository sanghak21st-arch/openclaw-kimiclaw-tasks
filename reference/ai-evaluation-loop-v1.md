# AI 평가루프 v1 설계

> 작성일: 2026-03-14
> 버전: v1.0
> 담당: CEO (클로이)
> 상태: 완료
> 관련 이슈: OPE-20

---

## 1. 개요

**목적:** 사업계획서 초안을 AI가 평가하여 심사 관점 약점을 파악하고 개선 방향을 제시

**핵심 가치:**
- 심사 통과 가능성 점수화 (100점 만점)
- 약점 영역 자동 탐지
- 구체적인 보완 가이드 제공

---

## 2. 평가 프레임워크

### 2-1. 평가 영역 (5대 핵심 영역)

| 영역 | 가중치 | 평가 항목 |
|------|--------|----------|
| **사업성** | 25% | 시장 규모, 성장성, 수익 모델 |
| **실현성** | 25% | 실행 계획, 로드맵, 팀 역량 |
| **차별성** | 20% | 경쟁 우위, 진입 장벽, 차별화 요소 |
| **완성도** | 20% | 문서 구성, 논리성, 근거 제시 |
| **적합성** | 10% | 지원사업 목적 부합도 |

### 2-2. 세부 평가 항목

#### 사업성 (25점)
- [ ] TAM/SAM/SOM 제시 (5점)
- [ ] 시장 성장성 근거 (5점)
- [ ] 수익 모델 명확성 (5점)
- [ ] 단기/중기/장기 목표 (5점)
- [ ] 사업성 검증 자료 (5점)

#### 실현성 (25점)
- [ ] 3/6/12개월 로드맵 (8점)
- [ ] 팀 구성 및 역량 (7점)
- [ ] 예산 계획 현실성 (5점)
- [ ] 리스크 대응 전략 (5점)

#### 차별성 (20점)
- [ ] 경쟁사 분석 (5점)
- [ ] 핵심 차별 요소 (7점)
- [ ] 지속 가능한 우위 (5점)
- [ ] 기술/특허/네트워크 (3점)

#### 완성성 (20점)
- [ ] 문서 구조 적절성 (5점)
- [ ] 논리적 흐름 (5점)
- [ ] 수치/근거 제시 (5점)
- [ ] 언어/표현 품질 (5점)

#### 적합성 (10점)
- [ ] 지원사업 목적 부합 (5점)
- [ ] 평가 기준 충족 (5점)

---

## 3. 평가 알고리즘

### 3-1. 점수 산정 로직

```python
def calculate_score(evaluation_data):
    """
    총점 = Σ(영역별 점수 × 가중치)
    """
    weights = {
        'business_viability': 0.25,
        'feasibility': 0.25,
        'differentiation': 0.20,
        'completeness': 0.20,
        'fitness': 0.10
    }
    
    total_score = 0
    for area, weight in weights.items():
        area_score = evaluation_data[area]['score']
        total_score += area_score * weight
    
    return round(total_score, 1)


def determine_grade(total_score):
    """
    등급 판정
    """
    if total_score >= 80:
        return 'A', '통과 가능성 높음'
    elif total_score >= 60:
        return 'B', '보완 후 통과 가능'
    elif total_score >= 40:
        return 'C', '상당한 보완 필요'
    else:
        return 'D', '전면 재검토 필요'
```

### 3-2. 약점 탐지 로직

```python
def detect_weaknesses(evaluation_data):
    """
    영역별 약점 자동 탐지
    """
    weaknesses = []
    thresholds = {
        'business_viability': 15,  # 25점 만점의 60%
        'feasibility': 15,
        'differentiation': 12,
        'completeness': 12,
        'fitness': 6
    }
    
    for area, threshold in thresholds.items():
        score = evaluation_data[area]['score']
        if score < threshold:
            weaknesses.append({
                'area': area,
                'current_score': score,
                'threshold': threshold,
                'gap': threshold - score,
                'priority': 'high' if score < threshold * 0.8 else 'medium'
            })
    
    return sorted(weaknesses, key=lambda x: x['gap'], reverse=True)
```

---

## 4. 평가루프 프로세스

### 4-1. 1차 평가 (초안 제출 시)

```
[사용자 입력] → [AI 초안 생성] → [1차 평가] → [결과 제공]
                                              ↓
                                        점수 + 약점 리포트
                                        + 보완 권장사항
```

**출력물:**
- 총점 및 등급
- 영역별 세부 점수
- 상위 3개 약점
- 보완 우선순위 리스트

### 4-2. 2차 평가 (보완 후)

```
[보완 입력] → [고도화 산출] → [2차 평가] → [최종 리포트]
                                           ↓
                                     개선점 비교
                                     + 제출 권장 여부
```

**출력물:**
- 개선된 점수
- 1차 vs 2차 비교
- 제출 적합성 판정
- 최종 체크리스트

### 4-3. 평가 주기

| 단계 | 시점 | 목적 |
|------|------|------|
| **1차** | 초안 생성 후 | 빠른 약점 파악 |
| **2차** | 보완 입력 후 | 개선 확인 및 제출 결정 |
| **3차** | 최종 제출 전 | 마지막 품질 체크 (선택) |

---

## 5. 프롬프트 설계

### 5-1. 평가 프롬프트 템플릿

```
당신은 정부지원사업 심사위원입니다. 다음 사업계획서를 평가해주세요.

[평가 영역]
1. 사업성 (25점): 시장 규모, 성장성, 수익 모델
2. 실현성 (25점): 실행 계획, 팀 역량, 예산 현실성
3. 차별성 (20점): 경쟁 우위, 차별화 요소
4. 완성성 (20점): 문서 구성, 논리성, 근거
5. 적합성 (10점): 지원사업 목적 부합도

[평가 기준]
- 각 영역별로 0~만점 사이 점수 부여
- 점수 부여 시 구체적인 근거 명시
- 약점 발견 시 개선 방향 제시

[출력 형식]
{
  "total_score": 숫자,
  "grade": "A/B/C/D",
  "area_scores": {
    "business_viability": {"score": 숫자, "feedback": "피드백"},
    "feasibility": {"score": 숫자, "feedback": "피드백"},
    "differentiation": {"score": 숫자, "feedback": "피드백"},
    "completeness": {"score": 숫자, "feedback": "피드백"},
    "fitness": {"score": 숫자, "feedback": "피드백"}
  },
  "top_weaknesses": [
    {"area": "영역", "issue": "문제", "suggestion": "개선방향"}
  ],
  "improvement_priority": ["우선순위1", "우선순위2", "우선순위3"]
}

[사업계획서 내용]
{{business_plan_content}}
```

### 5-2. 보완 가이드 프롬프트

```
다음 약점을 보완하기 위한 구체적인 가이드를 제공해주세요.

[약점 정보]
영역: {{weakness_area}}
문제: {{weakness_issue}}
현재 점수: {{current_score}}

[요청사항]
1. 구체적인 보완 방향 3가지 제시
2. 예시 문장 또는 데이터 포함
3. 예상 점수 상승폭 제시

[출력 형식]
{
  "improvement_guide": [
    {
      "action": "구체적 행동",
      "example": "예시",
      "expected_impact": "점수 상승폭"
    }
  ],
  "required_data": ["필요한 데이터1", "필요한 데이터2"],
  "estimated_new_score": 예상 점수
}
```

---

## 6. 출력물 명세

### 6-1. 평가 리포트 (JSON)

```json
{
  "evaluation_id": "uuid",
  "business_plan_id": "uuid",
  "version": "v1",
  "timestamp": "2026-03-14T12:00:00Z",
  "summary": {
    "total_score": 72.5,
    "grade": "B",
    "grade_description": "보완 후 통과 가능",
    "submission_recommended": true
  },
  "area_scores": {
    "business_viability": {
      "score": 20,
      "max_score": 25,
      "percentage": 80,
      "feedback": "시장 규모 제시가 구체적이나 성장성 근거 보강 필요"
    },
    "feasibility": {
      "score": 18,
      "max_score": 25,
      "percentage": 72,
      "feedback": "로드맵이 명확하나 팀 역량 증빙 자료 필요"
    },
    "differentiation": {
      "score": 14,
      "max_score": 20,
      "percentage": 70,
      "feedback": "차별점이 있으나 경쟁사 대비 우위가 불분명"
    },
    "completeness": {
      "score": 15,
      "max_score": 20,
      "percentage": 75,
      "feedback": "문서 구조가 양호하나 수치 근거 보강 필요"
    },
    "fitness": {
      "score": 5.5,
      "max_score": 10,
      "percentage": 55,
      "feedback": "지원사업 목적과의 연결성 강화 필요"
    }
  },
  "weaknesses": [
    {
      "rank": 1,
      "area": "fitness",
      "issue": "지원사업 목적 부합도 부족",
      "current_score": 5.5,
      "gap": 4.5,
      "priority": "high",
      "improvement_suggestion": "지원사업 공고의 핵심 목적과 사업의 연결성을 명시"
    }
  ],
  "improvement_plan": {
    "priority_actions": [
      "지원사업 목적 연결성 강화",
      "경쟁사 대비 우위 구체화",
      "팀 역량 증빙 자료 추가"
    ],
    "estimated_score_after_improvement": 85.0
  }
}
```

### 6-2. 사용자 대시보드 (요약)

```
┌─────────────────────────────────────┐
│  AI 평가 결과          등급: B     │
│  총점: 72.5/100                     │
├─────────────────────────────────────┤
│  사업성    ████████░░  20/25        │
│  실현성    ██████░░░░  18/25        │
│  차별성    ██████░░░░  14/20        │
│  완성성    ███████░░░  15/20        │
│  적합성    █████░░░░░   5.5/10     │
├─────────────────────────────────────┤
│  🔴 우선 보완: 적합성               │
│  🟡 다음 보완: 차별성               │
│  🟢 양호: 사업성                    │
├─────────────────────────────────────┤
│  [보완 가이드 보기]  [제출하기]    │
└─────────────────────────────────────┘
```

---

## 7. 검증 및 테스트

### 7-1. 테스트 케이스

| 케이스 | 입력 | 예상 등급 | 검증 항목 |
|--------|------|----------|----------|
| **우수 사례** | 완성도 높은 계획서 | A (80+) | 전 영역 고득점 |
| **보완 필요** | 일부 누락 | B (60-79) | 특정 영역 약점 |
| **부족 사례** | 다수 누락 | C/D (<60) | 복수 영역 약점 |
| **적합성 부족** | 내용은 좋으나 목적 불일치 | B/C | 적합성만 낮음 |

### 7-2. 정확도 검증

- **심사위원 평가와의 상관관계:** 0.8 이상 목표
- **통과/탈락 예측 정확도:** 85% 이상 목표
- **보완 후 점수 상승 예측:** ±5점 이내

---

## 8. 다음 액션

1. **즉시:** 평가 프롬프트 개발팀 전달
2. **1주:** 테스트 케이스로 정확도 검증
3. **2주:** 실제 사업계획서로 파일럿 테스트
4. **3주:** 평가 기준 및 가중치 미세 조정

---

*산출물 완료: AI 평가루프 v1 설계 (프레임워크 + 알고리즘 + 프롬프트 + 출력물 명세)*
