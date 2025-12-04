### 4-2. 코드 리팩토링 (중복 로직 공통화)

#### BEFORE
- 동일한 에러 처리 로직에서 `Map<String, Object> error` 생성 및 `errorMsg.add(error)` if/else 블록마다 반복,  
  수정 시 모든 분기 코드를 변경해야 하는 구조
  
```Java
int areaAr = ValidationVO.getAreaAr();
String menuNm = ValidationVO.getMenuNm();
menuNm = StringUtils.isBlank(menuNm)?"메뉴명":menuNm;

if(String.valueOf(areaAr) == "" || String.valueOf(areaAr) == null) {
	Map<String, Object> error = new HashMap<>();
	error.put("sggCd", ValidationVO.getSggCd());
	error.put("codeNm", ValidationVO.getCodeNm());
	error.put("menuNm", menuNm);
	error.put("errorMsg", String.format("%s의 면적을 입력해 주세요.", menuNm));
	errorMsg.add(error);
} else {
	if(String.valueOf(areaAr).length() <= 6) {
		Map<String, Object> error = new HashMap<>();
		error.put("sggCd", ValidationVO.getSggCd());
		error.put("codeNm", ValidationVO.getCodeNm());	
		error.put("menuNm", menuNm); 
		error.put("errorMsg", String.format("%s의 면적을 확인해 주세요. 최소 7자리 이상으로 입력되어야 합니다."
				,menuNm));
		errorMsg.add(error);
	}
}
```


#### AFTER
- 공통 에러 생성 로직을 `Consumer<String>`로 분리
- 불필요한 지역 변수 제거
- 중복 코드를 제거하고 가독성을 향상

```Java
final String menuNm = StringUtils.isBlank(ValidationVO.getMenuNm()) ? "메뉴명" : ValidationVO.getMenuNm();

Consumer<String> comError = msg -> {
Map<String, Object> error = new HashMap<>();
error.put("sggCd", ValidationVO.getSggCd());
error.put("codeNm", ValidationVO.getCodeNm());
error.put("menuNm", menuNm);
error.put("errorMsg", msg);
errorMsg.add(error);
};

if (areaAr == 0) {
comError.accept(String.format("%s의 면적을 입력해 주세요.", menuNm));
} else if (String.valueOf(areaAr).length() <= 6) {
comError.accept(String.format("%s의 면적을 확인해 주세요. 최소 7자리 이상으로 입력되어야 합니다.", menuNm));
}
```


#### Result
- 불필요 import 및 중복 로직 제거로 **76줄 → 61줄**, 약 20% 코드 감소
- 에러 처리 로직을 단일 함수로 관리하여 **중복 디버깅 최소화**
- Validation 관련 규칙 변경 시, 한 곳만 수정하면 되어 **유지보수 업무 효율이 향상됨**

