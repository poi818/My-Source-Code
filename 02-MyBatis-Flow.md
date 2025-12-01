## 🔧 MVC + MyBatis 구현 예시

### 1. Controller (예외처리)
```java
@GetMapping("/selectCode/selectCodeList.do")
public ModelAndView selectCodeList(CodeVO vo) throws Exception {
  try {
	 return jsonForm.modelAndViewJson("200", "success", codeService.selectCodeList(vo));
  } catch (SQLException e) {
	// DB 오류
	return jsonForm.modelAndViewJson("500", "error");
  } catch (Exception e) {
	// 서비스 오류
	return jsonForm.modelAndViewJson("500", "error");
  }
}
```

### 2. MyBatis Mapper XML
```xml
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sample.searchCode.mapper.searchCodeMapper">
	<select id="selectCodeList" resultType="CodeVO">
	/* selectCodeList */
	select
		code_id
		,code_nm
		,cret_dt
		,updt_dt
		,dlte_dt
	from COM_CODE	
	</select>
</mapper>
```

