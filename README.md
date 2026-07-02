# 키둥이 서비스 런칭 워크플랜 · 간트차트

키디키디 APP Add-on(LLM 큐레이팅 검색) **키둥이** 서비스 런칭(2026-09-01)을 위한
인터랙티브 간트차트 워크플랜입니다. 킥오프(6/24)부터 런칭 주(8/31~9/6)까지 11주,
11개 부문·약 95개 과업·연결관계·마일스톤·임계경로를 일단위로 관리합니다.

> ⚠️ **대외비** — 내부 조직·담당자·일정 정보를 포함합니다. URL을 외부에 공유하지 마세요.
> (검색엔진 비노출(noindex) 설정됨. 단, 저장소가 공개이므로 링크/소스는 접근 가능합니다.)

## 사용법
- **BAR 드래그**: 일정 이동 / **양끝 드래그**: 기간 조절 / **호버**: 상세 툴팁
- **BAR 클릭**: 우측 패널에서 과업명·일정·담당·성격·선행·메모 편집, 삭제
- **＋ 과업 추가**, **🔗 연결 모드**(선행→후행), **🎯 임계경로**(런칭 결정 체인)
- 각 BAR 안 **조직 배지**(버튼형)로 담당 조직 표시 · **부문 접기**(요약 BAR)
- **필터·검색·줌·색상 토글(조직/업무성격)**
- **📄 1P 요약표**: 10pt 맑은 고딕 표 → PPT 붙여넣기/PDF 인쇄

## 함께 이어서 작업하기 (실시간 공유 · Supabase)
데이터는 **Supabase**에 실시간 공유됩니다. **링크만 있으면 누구나 열람·편집**할 수 있고,
변경은 즉시 모두에게 반영됩니다(좌상단 **🟢 실시간 연결**). 별도 로그인/토큰이 필요 없습니다.

### 최초 1회 설정 (테이블 생성)
Supabase 대시보드 → **SQL Editor** → 아래 실행 (프로젝트: `ckabuiwrwdjnklpvgqya`):
```sql
create table if not exists public.gantt_state (
  id text primary key,
  data jsonb,
  updated_at timestamptz default now()
);
alter table public.gantt_state enable row level security;
create policy "public read"   on public.gantt_state for select using (true);
create policy "public insert" on public.gantt_state for insert with check (true);
create policy "public update" on public.gantt_state for update using (true) with check (true);
alter publication supabase_realtime add table public.gantt_state;
```
> 연결값(`index.html`의 `SYNC`): url=`https://ckabuiwrwdjnklpvgqya.supabase.co`,
> anonKey=publishable 키(클라이언트 공개용, 안전). 첫 접속자가 현재 데이터로 자동 시드합니다.

## 배포
GitHub Pages(main / root)로 서비스됩니다. 과업 기본값은 `index.html`의 `DEFAULT_TASKS`,
공유 데이터는 Supabase `gantt_state` 테이블에서 실시간 관리됩니다.
