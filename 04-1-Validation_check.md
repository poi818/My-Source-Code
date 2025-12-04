### 4-1. Validation 로직 전환 (JavaScript → Server)

#### BEFORE (JavaScript)
- 화면 스크립트에서만 검증이 이뤄져, 우회 가능성이 존재

```JavaScript
if($('#planDe1').val() === '') {
	$('#InputDoneTxt').html("계획일자를 선택해 주세요.");
	lay_pop_open('#js__InputDone');
	return;
}

var datetimeRegexp = /^\d{4}-(0[1-9]|1​)-(0[1-9]|​[0-9]|3​)$/;
if(!datetimeRegexp.test($('#planDe1').val())) {
	$('#InputDoneTxt').html("계획일자를 확인해 주세요." +"연도-월-일 형식으로 입력되어야 합니다. (예. 2024-12-31)
	" +"캘린더 아이콘 클릭 후 다시 선택해 주세요.");
	lay_pop_open('#js__InputDone');
	return;
}

if($('#planDe1').val().split('-') > $('#currentDate').val()) {
	$('#InputDoneTxt').html("계획일자는 입력 연도보다 미래일 수는 없습니다.");
	lay_pop_open('#js__InputDone');
	return;
}
```

#### AFTER (Server)
- 검증 로직을 서버 단으로 전환하여, 클라이언트 우회 방지 및 공통 검증 로직으로 재사용 가능
- 날짜 파싱과 연도 비교를 `LocalDate` 기반으로 수행해 안정성 향상.
- 공통 에러 생성 로직은 `Consumer<String>`으로 분리

```Java
String planDe = ValidationVO.getPlanDe();
final String menuNm = StringUtils.isBlank(ValidationVO.getMenuNm()) ? "메뉴명" : ValidationVO.getMenuNm();

Consumer<String> comError = msg -> {
Map<String, Object> error = new HashMap<>();
error.put("sggCd", ValidationVO.getSggCd());
error.put("menuCodeNm", ValidationVO.getMenuCodeNm());
error.put("menuNm", menuNm);
error.put("errorMsg", msg);
errorMsg.add(error);
};

if(StringUtils.isEmpty(planDe)) {
comError.accept(String.format("%s의 계획일자를 선택해 주세요.", menuNm));
} else {
	try {
		LocalDate planDate = LocalDate.parse(planDe, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
		int yearOfPlanDate = planDate.getYear();
		int currentYear = Integer.parseInt(Year); // Year는 외부에서 주입되는 기준 연도
		
		if(yearOfPlanDate > currentYear) {
			comError.accept(String.format("%s의 계획일자는 입력 연도보다 미래일 수는 없습니다.", menuNm));
		}
	} catch(Exception e) {
		comError.accept(String.format(
			"%s의 계획일자를 확인해 주세요. 연도-월-일 형식으로 입력되어야 합니다. (예. 2024-12-31) 캘린더 아이콘 클릭 후 다시 선택해 주세요.",
			menuNm
		));
	}
}
```

#### Result
- 클라이언트 단 검증 로직을 서버 공통 모듈로 전환하여 **우회 가능성 차단 및 일관된 검증 보장**
- 날짜 처리 로직을 표준 API(`LocalDate`)로 통일해 **가독성과 유지보수성 향상**
- 기존 JavaScript 팝업 메세지 대신, 서버 단에서 에러 정보를 구조화하여 **다양한 화면/서비스에서 재사용 가능**

