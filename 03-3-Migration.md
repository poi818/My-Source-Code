## 🔧 3-3 마이그레이션 전환

**Oracle → PostgreSQL (전체 쿼리 50% 변환)**

**Before (Oracle)**
```xml
select fn_get_main_cd(#{SggCd}) as mainCd from dual -- DUAL 더미테이블: 파서 5단계
```

**After (PostgreSQL)**
```xml
select fn_get_main_cd(#{SggCd}) as mainCd -- DUAL 제거: 파서 1단계 + 실행계획 단순화
 ```

**성과**: **423MB 테이블 실운영 30초↑ → 30초 이내 (I/O 에러 0건)** 
