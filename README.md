# 국토정보통합플랫폼(KLIP) 공무원시스템 (보안상 익명화)

## 📊 주요 업무
- **1. REST-SOAP 하이브리드**: 외부기관 연계 **100% 성공**
  - Frontend: REST API / **Server: Axis SOAP**
- **2. 분석/설계/구현**: **MVC 패턴 + MyBatis**
- **3. SQL 튜닝**:
  - 423MB 테이블: 실운영 **30초↑ → 30초 이내** (성능 안정화)
  - 630MB 테이블: 실운영 **30초↑ → 30초 이내** (성능 안정화)
  - Oracle → PostgreSQL 마이그레이션: **전체 쿼리 오류 0건**

## 🚀 소스 파일
- [01-REST-SOAP-Hybrid.md](01-REST-SOAP-Hybrid.md) **하이브리드 연동**
- [02-MyBatis-Flow.md](02-MyBatis-Flow.md) **MVC 전체 흐름**
- **03-SQL-Tuning:** **SQL 최적화**
  - **03-1: INDEX 미사용 + RIGHT 최적화** [03-1-SQL-Optimization.md](03-1-SQL-Optimization.md)
  - **03-2: SUBSTR→RIGHT (3배↑)** [03-2-Function.md](03-2-Function.md)
  - **03-3: 마이그레이션 전환** [03-3-Migration.md](03-3-Migration.md)


