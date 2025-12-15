## 🔧 3-3 마이그레이션 전환

**Oracle → PostgreSQL (전체 쿼리 약 50% 전환 담당)**

**Before (Oracle)**
```xml
-- DUAL 더미테이블: 파서 5단계
select fn_get_main_cd(#{SggCd}) as mainCd from dual 

select SYSDATE AS current_time FROM DUAL;

NVL(NVL(NVL(mobile_tel, home_tel), office_tel), '미등록')
```

**After (PostgreSQL)**
```xml
-- DUAL 제거: 파서 1단계 + 실행계획 단순화
select fn_get_main_cd(#{SggCd}) as mainCd 

SELECT CURRENT_TIMESTAMP AS current_time;

COALESCE(
    NULLIF(mobile_tel, ''),
    NULLIF(home_tel, ''),
    NULLIF(office_tel, ''),
    '미등록'
) 
```

#### Result
- 주요 조회 쿼리에서 **실행 계획 단순화 및 응답 속도 개선** (Oracle 대비 안정적인 성능 확보)
- I/O 에러 0건 유지
