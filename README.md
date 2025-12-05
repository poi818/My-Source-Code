# 국토이용정보통합플랫폼(KLIP) 공무원시스템 (보안상 익명화)

## 📊 주요 업무
- **1. REST-SOAP 하이브리드**: 외부기관 연계 **100% 성공**
  - **Frontend: REST API** / **Server: Axis SOAP**
- **2. 분석/설계/구현**
  - **MVC 패턴 + MyBatis (Back-End)**
  - **JSP + JavaScript (Front-End)**
- **3. SQL 튜닝**:
  - 423MB 테이블: 실운영 **30초↑ → 30초 이내** (I/O 에러 0건) 
  - 630MB 테이블: 실운영 **30초↑ → 30초 이내** (I/O 에러 0건)
  - Oracle → PostgreSQL 마이그레이션: **전체 쿼리 오류 0건**
- **4. 코드 품질·구조 개선**:
  - Validation 로직 변경 **JavaScript → Server** (검증 안정성 강화)
  - 기존 중복 코드 **공통 모듈화**

## 🚀 소스 파일
- [01-REST-SOAP-Hybrid.md](01-REST-SOAP-Hybrid.md) **하이브리드 연동**
- [02-MyBatis-Flow.md](02-MyBatis-Flow.md) **MVC 전체 흐름**
- **03-SQL-Tuning:** **SQL 최적화**
  - **03-1: INDEX 미사용 + RIGHT 최적화** [03-1-SQL-Optimization.md](03-1-SQL-Optimization.md)
  - **03-2: SUBSTR→RIGHT (3배↑)** [03-2-Function.md](03-2-Function.md)
  - **03-3: 마이그레이션 전환** [03-3-Migration.md](03-3-Migration.md)
- **04-Code_REFORM:** **코드 품질 개선 관리**
  - **04-1: 검증 로직 전환(JavaScript -> Server)** [04-1-Validation_Check.md](04-1-Validation_check.md)
  - **04-2: 중복코드 공통 모듈화** [04-2-Code_Module.md](04-2-Code_Module.md) 






