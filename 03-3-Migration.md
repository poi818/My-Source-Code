## 🔧 3-3 마이그레이션 전환

**Oracle → PostgreSQL (전체 쿼리 50% 변환)**

**Before (Oracle)**
```xml
-- DUAL 더미테이블: 파서 5단계
select fn_get_main_cd(#{SggCd}) as mainCd from dual 

select SYSDATE AS current_time FROM DUAL;
```

**After (PostgreSQL)**
```xml
-- DUAL 제거: 파서 1단계 + 실행계획 단순화
select fn_get_main_cd(#{SggCd}) as mainCd 

SELECT CURRENT_TIMESTAMP AS current_time; 
```

**성과**: I/O 에러 0건, 쿼리 성능 향상
