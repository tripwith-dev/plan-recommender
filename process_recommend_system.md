# 추천 알고리즘 프로세스

### 1. plan 연관 데이터 추출

#### 1-1. 관련 엔터티만 추출

데이터를 추출 전 현재 ERD에서 plan과 연관된 엔터티를 모두 찾아줍니다. trip with 프로젝트에서 plan과 연관된 엔터티는 다음과 같습니다.

![erd_plan](erd_plan.png)

`plan`, `plan_tag_mapping`, `plan_tag`, `plan_detail`, `location`, `location_type_mapping` 그리고 `location_type` 을 이용하여 해당 plan이 어느 태그를 지니고, 어느 여행지를 방문하는 지에 따라 벡터값을 구하기로 결정하였습니다.

> `avatar_like_plan`, `plan_comment` 엔터티도 있지만, 머신러닝이 아닌 콘텐츠 기반 추천 시스템이기에 제외하고 진행하였습니다.

#### 1-2. 관련 엔터티의 컬럼

-   **`plan`의 컬럼**:
    -   `createdAt`, `updatedAt`, `deletedAt`, `isDeleted`, `planId`, `planTitle`, `planMainImage`, `status`, `travelStartDate`, `travelEndDate`, `likesCount`, `totalPrice`, `categoryId`, `avatarId`
-   **`plan_tag`의 컬럼**:
    -   `pTagId`, `pTagName`
-   **`plan_detail`의 컬럼**:
    -   `createdAt`, `updatedAt`, `deletedAt`, `isDeleted`, `detailId`, `startTime`, `endTime`, `detailTitle`, `price`, `priceType`, `currency`, `notes`, `planId`, `locationId`
-   **`location`의 컬럼**:
    -   `locationId`, `placeId`, `locationName`, `address`, `latitude`, `longitude`, `locationRating`
-   **`location_type`의 컬럼**:
    -   `typeId`, `type`

### 2. 추출된 데이터에서 모든 plan의 벡터 생성

### 3. 사용자의 최근 생성 planId 조회

### 4. 해당 plan의 벡터를 기준 벡터로 선택

### 5. 나머지 plan 벡터들과 코사인 유사도 계산

### 6. 유사도 높은 순으로 추천 결과 반환
