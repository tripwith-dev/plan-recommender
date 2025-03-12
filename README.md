# 🧭 콘텐츠 기반 여행 플랜 추천 시스템 설계 문서

### 🎯 프로젝트 개요

-   사용자의 **최근 생성한 여행 플랜(plan)** 을 기반으로,
-   **유사한 다른 사용자들의 플랜**을 추천하는 콘텐츠 기반 추천 시스템입니다.

> 본 시스템은 **머신러닝 모델 학습 없이**, 실시간 벡터 유사도 계산 방식으로 작동합니다.

---

## 📌 추천 방식 개요

#### ✔ 콘텐츠 기반 필터링 (Content-Based Filtering)

-   각 `plan`은 **특징 벡터(feature vector)** 로 변환됩니다.
-   사용자 최근 생성한 `plan`의 벡터와 **다른 모든 plan 벡터들과 코사인 유사도**를 계산합니다.
-   유사도가 높은 순서대로 추천합니다.

---

### 🧠 특징 벡터 구성 (ml_plan_features View 기반)

| 필드명            | 설명                          |
| ----------------- | ----------------------------- |
| planId            | 플랜 고유 ID                  |
| tagText           | 플랜에 포함된 태그들의 문자열 |
| locationText      | 방문한 장소 이름 + 장소 유형  |
| durationDays      | 여행 기간 (일 수)             |
| locationRatingAvg | 장소 평균 평점                |
| totalLikes        | 해당 플랜의 좋아요 수         |

> ※ `tagText`와 `locationText`는 텍스트 벡터화 (TF-IDF 등) 처리

---

### 🧮 추천 알고리즘 흐름

1. `ml_plan_features`에서 **모든 plan의 벡터 생성**
2. 사용자의 **최근 생성 planId 조회**
3. 해당 plan의 벡터를 **기준 벡터로 선택**
4. 나머지 plan 벡터들과 **코사인 유사도 계산**
5. 유사도 높은 순으로 **추천 결과 반환**

---

### 📊 유사도 계산 예시 (코사인 유사도)

```python
from sklearn.metrics.pairwise import cosine_similarity

# 예시: 기준 벡터와 전체 벡터 비교
similarity_scores = cosine_similarity(target_vector, all_plan_vectors)
```
