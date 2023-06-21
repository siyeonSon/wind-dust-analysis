# 🌏풍향에 따른 미세먼지 농도 비교
문제 정의: 서풍이 부는 달은 미세먼지 농도가 상대적으로 높을 것이다

## 💨미세먼지
- data-source: [KOSIS 미세먼지(PM2.5) 월별 대기오염도(측정망별,시도별,도시별,측정지점별)](https://kosis.kr/statHtml/statHtml.do?orgId=106&tblId=DT_106N_03_0200076&vw_cd=MT_ZTITLE&list_id=T_7&seqNo=&lang_mode=ko&language=kor&obj_var_id=&itm_id=&conn_path=MT_ZTITLE)
- 기간: 2015/01/01 ~ 2021/12/31
    - 2022년까지 존재하지만 12월 데이터 부재로 제외

1. 데이터 가공
    - 년 별, 구 별로 데이터가 분리되어 있어서 하나의 데이터 프레임으로 연결
    - `*` `-` 특수 문자 제거
  
![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/a9318cb5-71b3-4543-b8ab-8a58f8aeeaaa)


2. 미세먼지가 가장 심한 달은?
    - 지역별로 미세먼지 평균이 가장 높은 월 구하기
    - **겨울에 미세먼지 농도가 높은 것을 확인할 수 있음**

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/c2014345-053d-4b25-8d5f-5f54f5456d35)

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/09916164-3aa7-49e4-9dfe-1901f414abb6)


3. 미세먼지가 가장 심한 지역은?
    - 2021년 전국 미세먼지 연평균
    - **서쪽에 위치한 지역(충남, 경기, 인천 등)이 동쪽에 위치한 지역(강원, 울산, 경남 등) 보다 미세먼지 농도가 높음**

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/f878159a-ee00-48af-94f8-a28a8be9e265)

---

## 💨풍향
- data-source: [기상청_지상(종관, ASOS) 일자료 조회서비스](https://www.data.go.kr/data/15059093/openapi.do)
- 기간: 2015/01/01 ~ 2021/12/31
    - 미세먼지 데이터와 통일

1. API 호출
    - stnNm : 지점명(종관기상관측 지점명)
    - tm : 일시
    - sumRn : 일강수량(mm)
    - avgWs : 평균 풍속(m/s)
    - maxWd : 최다 풍향(16방위)

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/c8681633-a94f-474e-b6fd-4fcf47bf9d95)


2. 날짜 별로 상세한 비교 및 분석을 위해 일시 데이터를 년/월/일로 분리

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/9099362d-d21e-4e4e-a218-9b492871efd7)


3. dataset 가공
    - 일강수량 결측치 채우기
    - 풍속과 풍향 결측치 제거
    - 년, 월, 일 열의 타입을 문자열 타입으로 변경
    - 최다풍향(16방위) 열의 타입을 int형으로 변경

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/42e076ef-cff5-40b5-8b99-4e93ab714fd4)



4. 모든 데이터 셋이 20150101 ~ 20211231 데이터를 모두 포함하는 것은 아니었음
    - 정확한 분석을 위해 데이터셋이 부족한 지점을 삭제함
    - 세종시의 경우 2019년와 2020년 날씨 데이터만 가져와진 것을 확인할 수 있음
    - 세종시 날씨 데이터 제거


5. 월 별 날씨 데이터 가공
    - 월 별 미세먼지 농도를 분석할 것임
    - 일 별 날씨 데이터(기존) → 월별 날씨 데이터(변경)

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/a656192b-3e17-424b-9e50-4ac0b3eeae9b)



6. 2021년 사계절 최다풍향
    - 16 방위표를 기준으로 나선형 그래프를 그림
  
        <img width="680" alt="Screenshot 2023-06-21 at 6 11 58 PM" src="https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/3c6bb250-65aa-4316-a2fd-a2100c4710b4">


    - 봄, 여름, 가을에는 북동풍이 불고, 겨울에는 북서풍이 부는 것을 확인할 수 있음

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/2149d792-b320-4076-857d-305bae775629)



---

## 📝분석

### 상관 관계 분석
- **미세먼지와 최다풍향(16방위)은 상관 관계가 높음**

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/33a7a22a-d408-4aaf-a1ef-85bd4f1eed0e)


### 풍향별 미세먼지 농도
- **북서풍이 불 때 미세먼지 농도가 높음**

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/c1cd9dd9-ad3b-40e4-be87-aa30d27b2985)

---

# ✅결론
‘서풍이 부는 달은 미세먼지 농도가 상대적으로 높을 것이다’ 는 참이다
