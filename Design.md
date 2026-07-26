# Ambient Harmony 디자인 명세서 (Design.md)
## ESP32 Snoopy Smart Home Dashboard

본 문서는 **Snoopy Smart Home Dashboard**의 디자인 시스템과 컴포넌트 스타일 규칙을 정의합니다. 본 명세는 부드러운 평온함을 담아낸 **라이트 소프트 네오모피즘(Neumorphic Soft UI)**을 기반으로 하여, 디지털 인터페이스와 실 물리 하드웨어 간의 시각적 및 Tactile(촉각적) 일체감을 유도합니다.

---

## 1. 브랜드 & 디자인 컨셉 (Brand & Style Concept)

이 디자인 시스템의 핵심 철학은 **"Digital Serenity (디지털 평온함)"**입니다. 기존 네오모피즘이 가질 수 있는 저대비(Low Contrast) 접근성 문제를 완벽히 극복하기 위해, 감각적인 입체 엘리베이션 레이어와 활기찬 컬러 액센트(Electric Blue, Amber, Emerald)를 결합하여 미적 만족감과 직관적인 가시성을 동시에 충족합니다.

모든 컴포넌트는 실제 거실 벽에 내장된 스마트 기기 패널처럼 부드럽고 매끄러운 3D 가전 하우징 질감을 표현하며, 각 컨트롤 노브와 버튼은 눌렀을 때 안으로 쏙 들어가는(Sunken) 즉각적인 햅틱 피드백을 연출합니다.

---

## 2. 디자인 컬러 토큰 (Design Color Tokens)

| 컬러명 | 컬러 토큰값 | 주요 사용 대상 |
| :--- | :--- | :--- |
| **Base Surface** | `#F1F5F9` | 메인 캔버스 배경 및 기본 네오모픽 카드 색상 |
| **Highlight Bright** | `#FFFFFF` | 네오모픽 이중 광원의 좌상단 하이라이트 반사광 |
| **Shadow Dark** | `#CBD5E1` | 네오모픽 이중 광원의 우하단 어두운 그림자 |
| **Border Accent** | `#E2E8F0` | 네오모픽 경계 가시성을 선명하게 하기 위한 흐린 보더선 |
| **Electric Blue (Primary)** | `#2563EB` | 블루투스 연결 상태 배지, 페어링 연결 버튼, 핵심 UI 포인트 |
| **Soft Amber (Secondary)** | `#F59E0B` | 조도 센서 동기화 지시등, LCD 백라이트 등 광원 조절 포인트 |
| **Emerald Green (Tertiary)** | `#10B981` | 부저 멜로디 재생, 정상 환경 수치 및 안정 신호 |
| **Text Main** | `#191C1E` | 메인 헤드라인 타이틀, 중요한 정보 및 수치 데이터 |
| **Text Muted** | `#434655` | 서브 캡션 설명글, 컴포넌트 설명 레이블 |

---

## 3. 타이포그래피 (Typography)

가독성을 극대화하기 위해 기하학적이고 유려한 산세리프 서체를 기반으로 계층을 구성합니다.

- **기본 영문 서체**: `Plus Jakarta Sans`, `Inter`, `sans-serif`
- **데이터 & 콘솔 서체**: `JetBrains Mono`, `monospace` (숫자 데이터 및 로그 터미널의 자간 보정용)
- **한국어 타이포그래피 최적화**: 한글 서체의 시각적 복잡도를 감안하여 줄높이(Line-height)를 **1.6**으로 넓게 정의하여 시야의 답답함을 해소합니다.

---

## 4. 입체 엘리베이션 가이드라인 (Elevation & Shadows)

네오모피즘 특유의 입체 볼륨감은 좌상단의 백색 빛과 우하단의 그림자 각도를 시뮬레이션하여 렌더링합니다.

### 4.1. Raised Plate (기본 튀어나온 카드 및 버튼)
카드 영역이나 동작을 기다리는 평상시 상태의 버튼 컴포넌트입니다.
```css
background: #f1f5f9;
border: 1px solid #e2e8f0;
border-radius: 16px; /* 1rem의 둥글기를 일괄 적용하여 부드러운 마감 묘사 */
box-shadow: 6px 6px 12px #cbd5e1, -6px -6px 12px #ffffff;
transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
```

### 4.2. Sunken Well (음각으로 패인 인풋 및 활성/클릭 버튼)
수치를 입력하는 텍스트 영역, 하단 콘솔 웰, 또는 사용자가 클릭하여 활성화된 상태의 버튼 플레이트입니다.
```css
background: #f1f5f9;
border: 1px solid #e2e8f0;
box-shadow: inset 4px 4px 8px #cbd5e1, inset -4px -4px 8px #ffffff;
```

---

## 5. 컴포넌트 세부 디자인 규칙 (Component Architecture)

### 5.1. 제어 버튼 (Tactile Hardware Buttons)
- 평상시에는 `Raised` 그림자로 돌출되어 마우스 호버 시 위로 미세하게 올라오며(`transform: translateY(-1.5px)`), 마우스 클릭 시 즉시 `Sunken` 그림자로 반전되며 눌려 들어가는 3D 피드백을 전달해야 합니다.
- **아이콘 디자인**: 각 핀 번호와 하드웨어 대상에 걸맞은 구글 Material Symbols 아이콘을 가운데 배치하고 각각의 브랜드 컬러 액센트(블루, 옐로우, 에메랄드)를 단색 포인트로 주입합니다.

### 5.2. 모바일 바 형태 탭 메뉴 (Bottom Tab Bar)
- 화면의 하단에 고정(`position: fixed`)되어 떠 있는 유리(Glassmorphic) 플레이트 구조를 지닙니다. 
- 배경에 투명도 90% 그레이(`rgba(247, 249, 251, 0.9)`)를 주어 후면 콘텐츠가 흐릿하게 번져 보이는 `backdrop-filter: blur(12px)` 글래스모피즘 효과를 조합해 차원(Depth)을 나눕니다.

### 5.3. OLED Snoopy 배너 디자인 (OLED Graphic Screen Card)
- 텍스트 위주의 카드 사이에서 생동감을 제공하기 위해, 단색 스누피 일러스트 PBM 배경 스킨을 씌우고 그 위에 어두운 오버레이 레이어(`linear-gradient`)를 가볍게 도포하여 시인성을 확보합니다. 
- 메시지 전송 버튼을 누를 시 3초간 로딩 회전 인디케이터가 활성화되며, 쿨다운 중에는 버튼 입력을 차단하여 하드웨어 병목을 방지합니다.
