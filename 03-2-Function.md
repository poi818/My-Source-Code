## 🔧 3-2 RIGHT 최적화

**Before (630MB 전체 스캔)**
```xml
SELECT
	(CASE
		WHEN substr(max(mainCd), 17, 4) IS NULL THEN '0'
        WHEN substr(max(mainCd), 17, 4) = '' THEN '0'
		ELSE substr(max(mainCd), 17, 4)
    END) as seq_no -- 3중연산 느림
FROM MAIN_CODE_ISSU_RGTR
```
		
**After (인덱스 + RIGHT 최적화)**
```xml
SELECT
	(CASE
		WHEN LENGTH(TRIM(RIGHT(COALESCE(max(mainCd),''),4))) = 0 THEN '0' -- 코드간결화 및 RIGHT 사용으로 속도 증진 --
		ELSE RIGHT(max(mainCd), 4)
	END) as seq_no -- RIGHT 3배↑
FROM MAIN_CODE_ISSU_RGTR
```

#### Result
- 630MB 테이블 실운영 30초↑ → 15초 이내 단축
- I/O 에러 0건
