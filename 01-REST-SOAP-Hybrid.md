## 🔧 REST API + SOAP 하이브리드 구현

### 1. Frontend (REST API - jQuery Ajax)

$('#SendApi').find('#Send').off('click').on('click', function() {
   let data = new FormData();
   data.append("apiCd", o.response.info.apiCd); 
   data.append("apiPlanCd", o.response.info.apiPlanCd); 
   data.append("sggCd", o.response.info.sggCd); 

   $.ajax({
      url : "/api/apiOutput/send.do"
      ,data : data
      ,type : "post"
      ,enctype : "multipart/form-data"
      ,processData : false // FormData 쿼리스트링 변환 방지
      ,contentType : false // 브라우저 자동 multipart 설정
      ,dataType : "json"
      ,success : function(res) {
        var data = res.message;
        var code = res.code;
         
        lay_pop_open('#apiSuccessSend');
      }
      ,error : function() {
		lay_pop_open('#apiFailSend');
      }
   });
});


### 2. Server (Spring Controller - SOAP 연계)

@RequestMapping(value = "/api/apiOutput/send.do")   
public ModelAndView apiSend (apiSendVO vo, ModelMap map) throws Exception {
   String code = HttpStatus.OK.toString();
   String message = HttpStatus.OK.getReasonPhrase();
   String response = HttpStatus.OK.getReasonPhrase();
   
   apiService.sendApi(vo);
   
   return jsonForm.modelAndViewJson(code, message, response);
}

### 3. SOAP Client (Axis 라이브러리)

org.apache.axis.client.Service service = new org.apache.axis.client.Service();
Call call = (Call) service.createCall();
call.setTimeout(50000);
call.setTargetEndpointAddress(apiurl);

// 첨부파일 전송
Class jafsf = JAFDataHandlerSerializerFactory.class;
Class jafdf = JAFDataHandlerDeserializerFactory.class;
Class cls = DataHandler.class;
         
QName qname = new QName("", "DataHandler");
         
call.registerTypeMapping(cls, qname, jafsf, jafdf, false);

call.setOperationName(new QName("", setFileMethodName));
