# 🌏풍향에 따른 미세먼지 농도 비교
문제 정의: 서풍이 부는 달은 미세먼지 농도가 상대적으로 높을 것이다

## 💨미세먼지
- data-source: [KOSIS 미세먼지(PM2.5) 월별 대기오염도(측정망별,시도별,도시별,측정지점별)](https://kosis.kr/statHtml/statHtml.do?orgId=106&tblId=DT_106N_03_0200076&vw_cd=MT_ZTITLE&list_id=T_7&seqNo=&lang_mode=ko&language=kor&obj_var_id=&itm_id=&conn_path=MT_ZTITLE)
- 기간: 2015/01/01 ~ 2021/12/31
    - 2022년까지 존재하지만 12월 데이터 부재로 제외

1. 데이터 가공
    - 년 별, 구 별로 데이터가 분리되어 있어서 하나의 데이터 프레임으로 연결
    - `*` `-` 특수 문자 제거
  
![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/a67059cd-7e3c-49c7-b5f7-9c9d3c8a925f)


2. 미세먼지가 가장 심한 달은?
    - 지역별로 미세먼지 평균이 가장 높은 월 구하기
    - **겨울에 미세먼지 농도가 높은 것을 확인할 수 있음**

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/e3bc3dcd-037e-4138-974e-5b3bcf998dc2)

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/314ab1e1-e056-4907-a10b-4a0fd09a7b41)


3. 미세먼지가 가장 심한 지역은?
    - 2021년 전국 미세먼지 연평균
    - **서쪽에 위치한 지역(충남, 경기, 인천 등)이 동쪽에 위치한 지역(강원, 울산, 경남 등) 보다 미세먼지 농도가 높음**

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/5db3b07c-c532-47a0-9362-e0c69c1dc010)

---

## 💨**공공데이터 날씨 API**
- data-source: [기상청_지상(종관, ASOS) 일자료 조회서비스](https://www.data.go.kr/data/15059093/openapi.do)

1. API 호출
    - stnNm : 지점명(종관기상관측 지점명)
    - tm : 일시
    - sumRn : 일강수량(mm)
    - avgWs : 평균 풍속(m/s)
    - maxWd : 최다 풍향(16방위)

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/46a11e09-0b63-46c4-aa80-85272291e7c7)


2. 날짜 별로 상세한 비교 및 분석을 위해 일시 데이터를 년/월/일로 분리

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/c1322d16-0795-454f-b6eb-2567ec5e7a4c)


3. dataset 가공
    - 일강수량 결측치 채우기
    - 풍속과 풍향 결측치 제거
    - 년, 월, 일 열의 타입을 문자열 타입으로 변경
    - 최다풍향(16방위) 열의 타입을 int형으로 변경

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/3153419d-97e5-4d92-8b26-3bd5322b0db2)


4. 모든 데이터 셋이 20150101 ~ 20211231 데이터를 모두 포함하는 것은 아니었음
    - 정확한 분석을 위해 데이터셋이 부족한 지점을 삭제함
    - 세종시의 경우 2019년와 2020년 날씨 데이터만 가져와진 것을 확인할 수 있음
    - 세종시 날씨 데이터 제거


5. 월 별 날씨 데이터 가공
    - 월 별 미세먼지 농도를 분석할 것임
    - 일 별 날씨 데이터(기존) → 월별 날씨 데이터(변경)

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/7f633a0d-18b1-40ef-b545-c4f9c374505d)


6. 2021년 사계절 최다풍향
    - 16 방위표를 기준으로 나선형 그래프를 그림
  
        <img width="688" alt="Screenshot 2023-06-21 at 6 07 01 PM" src="https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/9740631f-2951-47e4-aed2-4d6e0025cf58">

    - 봄, 여름, 가을에는 북동풍이 불고, 겨울에는 북서풍이 부는 것을 확인할 수 있음

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/9564f200-2785-4cd9-8f91-06aa2d4504a2)


---

## 📝분석

### 상관 관계 분석
- **미세먼지와 최다풍향(16방위)은 상관 관계가 높음**

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/2dc13c12-0fc5-4bb0-abda-d71ebc60624b)


### 풍향별 미세먼지 농도
- **북서풍이 불 때 미세먼지 농도가 높음**

![image](https://github.com/siyeonSon/wind-dust-analysis/assets/87802191/4f479fb9-eaa1-49cf-ad34-c3afc26d61e8)

---

# ✅결론
‘서풍이 부는 달은 미세먼지 농도가 상대적으로 높을 것이다’ 는 참이다
