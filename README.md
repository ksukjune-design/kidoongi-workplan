# 키둥이 서비스 런칭 워크플랜 · 간트차트

키디키디 APP Add-on(LLM 큐레이팅 검색) **키둥이** 서비스 런칭(2026-09-01)을 위한
인터랙티브 간트차트 워크플랜입니다. 킥오프(6/24)부터 런칭 주(8/31~9/6)까지 11주,
11개 부문·약 95개 과업·연결관계·마일스톤을 일단위로 관리합니다.

> ⚠️ **대외비** — 내부 조직·담당자·일정 정보를 포함합니다. URL을 외부에 공유하지 마세요.
> (검색엔진 비노출(noindex) 설정되어 있으나, URL을 아는 사람은 접근·편집할 수 있습니다.)

## 사용법
- **BAR 드래그**: 일정 이동 / **양끝 드래그**: 기간 조절
- **BAR 클릭**: 우측 패널에서 과업명·일정·담당·성격·선행·메모 편집, 삭제
- **＋ 과업 추가**, **🔗 연결 모드**(선행→후행 클릭으로 화살표), **🎯 임계경로**(런칭 결정 체인)
- **부문 헤더 클릭**: 접기(요약 BAR) / **필터·검색·줌·색상 토글**
- **📄 1P 요약표**: 10pt 맑은 고딕 표로 내보내 PPT 붙여넣기/PDF 인쇄

## 함께 이어서 작업하기 (실시간 공동편집)
데이터를 **Supabase**(무료)에 두면 접속한 모든 사람이 **같은 데이터를 실시간으로** 편집합니다.
(뷰 설정 — 필터·접기·색상·줌 — 은 각자 개인 브라우저에 저장됩니다.)

### 최초 1회 설정
1. https://supabase.com 에서 무료 프로젝트 생성 → **Project URL**과 **anon public key** 확인
   (Project Settings → API).
2. Supabase 대시보드의 **SQL Editor**에서 아래 실행:
   ```sql
   create table if not exists gantt_state (
     id text primary key,
     data jsonb,
     updated_at timestamptz default now()
   );
   alter table gantt_state enable row level security;
   create policy "anon read"   on gantt_state for select using (true);
   create policy "anon insert" on gantt_state for insert with check (true);
   create policy "anon update" on gantt_state for update using (true) with check (true);
   -- 실시간 반영
   alter publication supabase_realtime add table gantt_state;
   ```
3. `index.html` 상단 `const SYNC = { ... }` 의 `url`, `anonKey` 를 채우고 커밋/푸시.
   → 저장하면 좌상단 상태가 **🟢 실시간 연결** 로 바뀌고, 첫 접속자의 데이터가 자동 시드됩니다.

설정 전에는 **🟡 로컬 저장**(각자 브라우저) 모드로 동작합니다.

## 배포 (GitHub Pages)
이 폴더를 저장소 루트로 하여 GitHub Pages(main / root)로 서비스됩니다.
과업 데이터 기본값은 `index.html` 내 `DEFAULT_TASKS` 배열에서 관리합니다.
