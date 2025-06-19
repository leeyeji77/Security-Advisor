## Security > Security Advisor > API 가이드
Security Advisor Public API를 설명 합니다.
## 공통 준비 사항
API 사용을 위해서는 API 엔드포인트와 토큰이 필요합니다.

### API 엔드포인트
| 리전 | 엔드포인트 |
| --- | ----- |
| 모든 리전 | [https://advisor.api.nhncloudservice.com](https://advisor.api.nhncloudservice.com/) |

### 인증 토큰 발급
Seucirty Advisor는 API 인증/인가를 받기 위해 NHN Cloud 토큰을 이용합니다.
[NHN Cloud API 호출 및 인증](https://docs.nhncloud.com/ko/nhncloud/ko/public-api/api-authentication/)을 확인하셔서 인증 토큰 사용에 필요한 정보를 확인합니다.
## API 사용 공통 정보

### API 요청 공통 정보

API를 사용하려면 다음과 같은 정보가 필요합니다.

* 토큰 발급 이후에 API 헤더에 토큰 정보를 포함시킵니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| x-nhn-authorization | Header | String | O | 토큰 |
* 서비스 Appkey
    * Security Advisor 콘솔 상단 **URL \& Appkey** 메뉴에서 확인하거나 프로젝트 관리의 **이용 중인 서비스**에서 확인할 수 있습니다.
    * 서비스 URL Path에 Appkey 가 포함됩니다.

### API 응답 공통 정보

* API 요청에 대한 응답으로 아래와 같이 응답 코드를 반환 할 수 있습니다.
    * **200 OK**
    * **400 Bad Reuqest**
    * **404 Not Found**
    * **405 Method Not Allowed**
    * **500 Internal Server Error**
* 모든 응답 코드는 공통의 response body를 포함합니다.
    * 공통 response body

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| header | Body | Object |  |
| header.isSuccessful | Body | Boolean | true: 정상<br>false: 오류 |
| header.resultCode | Body | Integer | 1: 정상<br>그 외: 오류 |
| header.resultMessage | Body | String | "SUCCESS": 정상<br>그 외: 오류 원인 메시지 |

* <span style="color:rgb(49, 51, 56);">공통 response body 외 자세한 응답 결과는 응답 본문 헤더를 참고합니다.</span>

> [주의] API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

## Security Advisor

### 최종 점검 날짜 조회

최종 점검 날짜를 조회합니다.

```
GET "/advisor/v1.0/appKey/{appKey}/last_inspection_date"
```
##### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| appKey | URL | String | O | 서비스 Appkey |

##### 응답

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| latestInspectionTime | Body | String | O | 최종 점검 날짜 (datetime) |

<details>
  <summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 1,
        "resultMessage": "Request success",
        "isSuccessful": true
    },
    "body": {
        "latestInspectionTime": "2025-03-11T16:00:32+09:00"
    }
}
```

<br>
</details>

### 자동 점검 설정 조회

관리자가 설정한 자동 점걸 설정 값을 조회합니다.

```
GET "/advisor/v1.0/appKey/{appKey}/setting"
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| appKey | URL | String | O | 서비스 Appkey |

#### 응답

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| <span style="">emails</span> | Body | <span style="">Array</span> | O | <span style="">점검 완료시 완료 메일을</span><br><span style="">수신 할 이메일 주소 리스트</span> |
| <span style="">isEnableAutoInspect</span> | Body | <span style="">Boolean</span> | O | <span style="">자동 점검 설정 여부</span><br><span style="">(해당 값이 false면 나머지 설정 무시)</span> |
| <span style="">inspectionList</span> | Body | <span style="">array</span> | X | <span style="">자동 점검 진행시 선택한 항목</span> |
| <span style="">inspectionCycle</span> | Body | <span style="">object</span> | X | <span style="">점검 주기 설정</span> |
| <span style="">inspectionCycle.isWeek</span> | Body | <span style="">boolean</span> | X | <span style="">주 단위를 선택했는지 여부</span> |
| <span style="">inspectionCycle.time</span> | Body | <span style="">string(hh:mm)</span> | X | <span style="">점검 진행 할 시간(00:00)</span> |
| <span style="">inspectionCycle.day</span> | Body | <span style="">integer</span> | X | <span style="">점검 진행 할 요일(일요일이 1부터 시작)</span> |
| <span style="">isWhole</span> | Body | <span style="">boolean</span> | X | <span style="">전체 점검 선택 여부</span> |

<details>
  <summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 1,
        "resultMessage": "Request success",
        "isSuccessful": true
    },
    "body": {
        "emails": ["nhncloud@nhn.com"],
        "isEnableAutoInspect": true,
        "inspectionList": [
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
        ],
        "inspectionCycle": {
            "isWeek": true,
            "time": "00:00",
            "day": 2
        },
        "isWhole": false
    }
}
```

<br>
</details>

### 마지막 점검 결과 요약 조회

마지막 점검 결과에 대한 요약 정보를 조회 합니다.

```
GET "/advisor/v1.0/appKey/{appKey}/inspection_results?region={region}&lang={lang}&page={page}&size={size}"
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| appKey | URL | String | O | 서비스 Appkey |
| region | Query | String | O | 리전 코드(KR1: 판교, KR2: 평촌,<br>JP1: 일본, US1: 미국) |
| lang | Query | Sring | O | 언어 코드(KO: 한글, EN: 영어, JA: 일본어) |
| page | Query | Integer | O | 조회할 페이지 번호 |
| size | Query | Integer | O | 조회할 페이지 크기 |

#### 응답

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| ruleNo | Array Body | integer | O | 점검 항목 번호 |
| status | Array Body | string | O | 점검 결과(critical: 위험, warning: 주의, interest: 관심, good: 양호) |
| inspectRange | Array Body | string | O | 점검 대상 범위(PROJECT: 프로젝트, ORG: 조직) |
| inspectContent | Array Body | string | O | 점검 대상 제목 |
| detectionCount | Array Body | integer | O | 탐지 개수 |
| exceptionCount | Array Body | integer | O | 예외 개수 |
| inspectTime | Array Body | string | O | 점검 시간(datetime) |

<details>
  <summary>예시</summary>
<p>

```json
{
    "header": {
        "resultCode": 1,
        "resultMessage": "Request success",
        "isSuccessful": true
    },
    "body": [
        {
            "ruleNo": 10,
            "status": "good",
            "inspectRange": "PROJECT",
            "inspectContent": "Security Groups 점검",
            "detectionCount": 0,
            "exceptionCount": 0,
            "inspectTime": "2023-06-12T19:53:35+09:00"
        },
        {
            "ruleNo": 11,
            "status": "good",
            "inspectRange": "PROJECT",
            "inspectContent": "Database Security Groups 점검",
            "detectionCount": 0,
            "exceptionCount": 0,
            "inspectTime": "2023-06-12T19:53:35+09:00"
        },
        {
            "ruleNo": 1,
            "status": "critical",
            "inspectRange": "ORG",
            "inspectContent": "IAM 로그인 실패 보안 점검",
            "detectionCount": 1,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:42+09:00"
        },
        {
            "ruleNo": 2,
            "status": "critical",
            "inspectRange": "ORG",
            "inspectContent": "IAM 로그인 세션 시간 점검",
            "detectionCount": 1,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:42+09:00"
        },
        {
            "ruleNo": 3,
            "status": "critical",
            "inspectRange": "ORG",
            "inspectContent": "IAM 로그인 세션 수 점검",
            "detectionCount": 1,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:42+09:00"
        },
        {
            "ruleNo": 4,
            "status": "good",
            "inspectRange": "ORG",
            "inspectContent": "프로젝트 멤버의 미사용 IAM 계정 점검",
            "detectionCount": 0,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:43+09:00"
        },
        {
            "ruleNo": 5,
            "status": "good",
            "inspectRange": "ORG",
            "inspectContent": "프로젝트의 IAM 계정 사용 여부 점검",
            "detectionCount": 0,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:43+09:00"
        },
        {
            "ruleNo": 6,
            "status": "critical",
            "inspectRange": "ORG",
            "inspectContent": "프로젝트 멤버 회원 계정의 2차 인증 설정 점검",
            "detectionCount": 1,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:43+09:00"
        },
        {
            "ruleNo": 7,
            "status": "critical",
            "inspectRange": "ORG",
            "inspectContent": "프로젝트 멤버 IAM 계정의 2차 인증 설정 점검",
            "detectionCount": 1,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:43+09:00"
        },
        {
            "ruleNo": 8,
            "status": "good",
            "inspectRange": "ORG",
            "inspectContent": "IAM 콘솔 도메인 설정 점검",
            "detectionCount": 0,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:43+09:00"
        },
        {
            "ruleNo": 9,
            "status": "critical",
            "inspectRange": "ORG",
            "inspectContent": "IAM 콘솔의 IP ACL 설정 점검",
            "detectionCount": 1,
            "exceptionCount": 0,
            "inspectTime": "2025-03-11T16:00:43+09:00"
        },
        {
            "ruleNo": 12,
            "inspectRange": "PROJECT",
            "inspectContent": "RDS 접근제어 점검",
            "detectionCount": 0,
            "exceptionCount": 0
        },
        {
            "ruleNo": 13,
            "inspectRange": "PROJECT",
            "inspectContent": "NCR 이미지 취약점 점검 설정 점검",
            "detectionCount": 0,
            "exceptionCount": 0
        }
    ]
}
```

</details>

